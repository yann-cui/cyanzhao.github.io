---
layout: post
title:  "golang 实现并发的两种方案"
date:   2021-09-29 18:01:50 +0800
categories: golang
---

本次我们实现一个简单的函数记忆问题，即缓存函数的结果，达到多次调用但只计算一次的效果。

我们的方案应该是并发安全的，并且要避免简单的对整个缓存使用单个锁而带来的锁争夺问题。

介绍两种推荐的方案来构建并发结构：共享变量上锁、通信顺序进程

---

* 示例场景

    在一个爬虫框架下，我们要并发的去访问一系列网址。
    
    ```
    func httpGetBody(url string)(interface{}, error){
        resp, err := http.Get(url)
        if err := nil{
            return nil, err
        }
        defer resp.Body.Close()
        return ioutil.ReadAll(resp.Body)
    }
    ```

    这些地址如果有重复的，应该直接使用该地址的最近一次爬取结果。
    所以需要对每次的爬取结果做一下缓存（暂不讨论缓存的过期机制）。

    ```
    // CrawlerFunc 一次爬取的函数定义
    type CrawlerFunc(key string) (interface{}, error)
    ```

    ```
    type result struct{
        value interface{}
        err error
    }

    // Memo 缓存了爬取的结果 
    type Memo struct {
        f CrawlerFunc
        cache map[string]result
    }

    func New(f CrawlerFunc) *Memo {
        return &Memo{f: f, cache: make(map[string]result)}
    }

    func (m *Memo)Get(key string) (interface{}, error){
        res, ok := m.cache[key]
        if !ok{
            res.value, res.err = m.f(key)
            m.cache[key] = res
        }
        return res.value, res.err
    }
    ```
    
    很明显的，当有两个或多个 goroutine 进入 Get 方法时，不是并发安全的。

    最简单的方法是给 Memo 实例增加一个互斥量，并在Get函数开头获取互斥锁，在返回前释放互斥锁。

    ```
    type Memo struct {
        f CrawlerFunc
        mu sync.Mutex
        cache map[string]result
    }

    func (m *Memo)Get(key string) (interface{}, error){
        m.mu.Lock()
        res, ok := m.cache[key]
        if !ok{
            res.value, res.err = m.f(key)
            m.cache[key] = res
        }
        m.mu.UnLock()
        return res.value, res.err
    }
    ```

    该方案是并发安全的。但由于每次调用f都会上锁，使我们的并行I/O操作串行化了。达不到我的并发需求。
    
    当然也可以在获取缓存和函数查询间分别上两次锁，这可以提高我们的并发度，但依然可能会发生我的查询函数被调用两次的情形。理想情况下，我们希望避免这种额外的处理。

    这是一个`重复抑制`的功能。我们考虑使用通道来实现，即读取缓存结果值时先通过通道来获取消息。

    对于缓存值的存取：可以在不同的goroutine中存取，但在操作前需要给缓存值上互斥锁，也可以在一个监控gorotine中实现缓存值的查询，然后同通信去进行结果的传递。

    下次分别介绍两种方式。

* 共享变量上锁

    ```
    type entry struct{
        res result
        ready chan struct{} // res 准备好后关闭
    }

    type New(f CrawlerFunc) *Memo{
        return &Memo{f: f, cache: make(map[string]*entry)}
    }

    type Memo struct {
        f   CrawlerFunc
        mu  sync.Mutex
        cache   map[string]*entry
    }

    func (m *Memo)Get(key string) (interface{}, error){
        m.mu.Lock()
        e := m.cache[key]
        if e == nil {
            // 对key的第一次访问
            // 这个goroutine负责计算数据和广播数据
            e = &entry{ready: make(chan struct{})}
            m.cache[key] = e
            m.mu.Unlock()

            e.rea.value, e.rea.err = m.f(key)

            close(e.ready) // 广播数据已经准备完毕的消息
        }else{
            // 对这个key的重复访问
            m.mu.Unlock()

            <-e.ready // 等待数据准备完毕
        }
        return e.res.value, e.res.err
    }
    ```

    可以看到互斥锁加在了 key 对应的 entry 实例上。当有其他goroutine进入Get时，需要等待第一次goroutine的查询结果。该结果通过关闭 e.ready 通道的方式进行广播。


* 通信顺序进程

    ```
    func (e *entry) call(f wantSomeFunc, key string) {
        e.res = result{
            value: f(key),
            err:   nil,
        }
        close(e.ready)
    }

    func (e *entry) deliver(response chan<- result) {
        <-e.ready
        response <- e.res
    }
    
    type request struct {
        key     string
        response    chan<- result
    }

    type Memo struct { requests chan request}

    func New(f CrawlerFunc) *Memo {
        m := &Memo{requests: make(chan request)}
        go m.server(f)
        return m
    }

    func (m *Memo) server(f CrawlerFunc) {
        cache := make(map[string]*entry)
        for r := range m.requests {
            e := cache[r.key]
            if e == nil {
                e = &entry{ready: make(chan struct{})}
                cache[r.key] = e
                go e.call(f, r.key)
            }
            go e.deliver(r.response)
        }
    }

    func (m *Memo) Get(key string) (interface{}, error) {
        response := make(chan result)
        m.requests <- request{key, response}
        res := <-response
        return res.value, res.err
    }

    func (m *Memo) Close() error {
        close(m.requests)
        return nil
    }  
    ```    

    与基于互斥锁的方案相似，对于指定key的一次请求结果保存在 entry 中，并通过关闭 ready 通道来广播数据准备状态。

    不同的是，请求被限制在一个监控goroutine中（即(*Memo).server）。
    监控goroutine从request通道中循环读取，写入response通道，直到该通道关闭。

    Get方法创建了一个response通道，放到请求里面，随后发送给监控goroutine，在马上从response通道读取。