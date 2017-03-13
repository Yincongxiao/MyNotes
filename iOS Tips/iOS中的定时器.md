###iOS中的定时器
* iOS开发中实现定时器可以有三种方案:
    - `performSelector: withObject: afterDelay:`                      
    -  GCD中的`void
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
* 如果调用NSTimer的`+ (NSTimer *)timerWithTimeInterval:(NSTimeInterval)ti target:(id)aTarget selector:(SEL)aSelector userInfo:(nullable id)userInfo repeats:(BOOL)yesOrNo;`会有内存泄露的风险.首先,timer在创建以后线程的runloop会持有这个timer,而timer会持有target对象,timer只有在收到`- invalidate`方法以后才会释放,并且释放target对象,所以如果我们在target的delloc方法里面调用[timer invalidate];是没有效果的,是因为该对象没有机会调用自己的delloc方法,解决这种问题,我们必须在delloc之外的其他时机来调用invalidate方法来宝正target的释放,

#### dispatch_after()
* 系统会帮我们处理线程级的逻辑，这样也我们更易于享受系统对线程所做的优化。除此之外，我们不用关心runloop的问题。并且调用的对象也不会被强行持有，这样上述的内存问题也不复存在。当然，需要注意block会持有其传入的对象，但这可以通过weakself解决。所以在这种延迟操作方案中，使用dispatch_after更佳。
* 但是，dispatch_after有个致命的弱点：dispatch_after一旦执行后，就不能撤销了。而performSelector可以使用cancelPreviousPerformRequestsWithTarget方法撤销，NSTimer也可以调用invalidate进行撤销。（注意：撤销任务与创建timer任务必须在同一个线程，即同一个runloop）