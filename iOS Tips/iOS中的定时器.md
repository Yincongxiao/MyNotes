###iOS中的定时器
* iOS开发中实现定时器可以有三种方案:
    - `performSelector: withObject: afterDelay:`                      
    - GCD中的`void
dispatch_after(dispatch_time_t when,
	dispatch_queue_t queue,
	dispatch_block_t block);`
    - NSTimer
    
* 其中使用非GCD的其他两种方法都是基于线程的runloop实现的,所以如果在主线程中执行任务,是可以正常执行的,因为主线程中的runloop是系统帮我们默认开启(run起来)的,但是子线程中的runloop系统并不会自动开启,所以在子线程中调用这两个方法我们需要手动将该线程的runloop跑起来,才能保证方法的正确执行.
* `NSTimer`和`preferSelector`方法的创建和撤销操作必须放在同一个线程中

####NSTimer
* NSTimer操作简单,但是问题是最多的.

```objc
//此方法需要手动的调用 runloop的`- (void)addTimer:(NSTimer *)timer forMode:(NSRunLoopMode)mode;`方法放到线程中,当然这里可以调配timer的优先级.
+ (NSTimer *)timerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation *)invocation repeats:(BOOL)yesOrNo;
+ (NSTimer *)scheduledTimerWithTimeInterval:(NSTimeInterval)ti invocation:(NSInvocation *)invocation repeats:(BOOL)yesOrNo;
```
