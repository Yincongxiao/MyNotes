##YYCache  tips
* 介绍:略过,直接去[github](https://github.com/ibireme/YYCache)中看详细的介绍和使用方法,本文主要写自己在学习过程中的收货总结
* iOS中为了防止多线程对资源的抢夺,所有开发时使用锁来保证线程的安全,在[这篇文章](http://blog.ibireme.com/author/ibireme/)中作者详细介绍了iOS中几种锁的性能对比,在YYCache中采用的是pthread_mutex.

 ![iOS中的锁](http://blog.ibireme.com/wp-content/uploads/2016/01/lock_benchmark.png)
* **当LRU**的实现:*It uses LRU (least-recently-used) to remove objects; NSCache's eviction method*,在YYMemeryCache中使用了lru规则来进行缓存的淘汰,当发生内存警告或者缓存值达到上限,会优先淘汰哪些时间戳靠前的对象,最近使用的不被淘汰.