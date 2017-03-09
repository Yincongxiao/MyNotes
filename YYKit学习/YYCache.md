##YYCache  tips
* 介绍:略过,直接去[github](https://github.com/ibireme/YYCache)中看详细的介绍和使用方法,本文主要写自己在学习过程中的收货总结
* iOS中为了防止多线程对资源的抢夺,所有开发时使用锁来保证线程的安全,在[这篇文章](http://blog.ibireme.com/author/ibireme/)中作者详细介绍了iOS中几种锁的性能对比,在YYCache中采用的是pthread_mutex.

 ![iOS中的锁](http://blog.ibireme.com/wp-content/uploads/2016/01/lock_benchmark.png)
 
 ####LRU淘汰算法
* **当LRU**的实现:*It uses LRU (least-recently-used) to remove objects; NSCache's eviction method*,在YYMemeryCache中使用了lru规则来进行缓存的淘汰,当发生内存警告或者缓存值达到上限,会优先淘汰哪些时间戳靠前的对象,最近使用的不被淘汰.YYMemoryCache中两个重要的内部对象`_YYLinkedMap`,`_YYLinkedMapNode`
#####_YYLinkedMapNode
* _YYLinkedMapNode 是缓存系统中的最小单元,直接被`_YYLinkedMap`所持有,先来看看这个类的声明

```objc
@interface _YYLinkedMapNode : NSObject {
    @package
    __unsafe_unretained _YYLinkedMapNode *_prev; // retained by dic
    __unsafe_unretained _YYLinkedMapNode *_next; // retained by dic
    id _key; //锁存对象的key
    id _value; //具体存储的对象
    NSUInteger _cost; // 所存对象占用空间
    NSTimeInterval _time; // 最近一次使用该对象的时间戳
}
```
也就是这个对象中拥有了一个被存储对象全部的信息,key,值,以及在linkMap中的location,location的实现是通过持有前一个对象的指针以及后一个对象的指针来实现的
#####_YYLinkedMap
* _YYLinkedMap是实现lru的关键,是(_YYLinkedMapNode *)的集合,这个集合管理了对象的出栈,入栈以及排序,相关的方法依次有
/// Remove a inner node and update the total cost.
/// Node should already inside the dic.
- (void)removeNode:(_YYLinkedMapNode *)node;

/// Remove tail node if exist.
- (_YYLinkedMapNode *)removeTailNode;

/// Remove all node in background queue.
- (void)removeAll
```