####记录iOS学习中的一些容易忘记但是很重要的tips,以备以后不定期的查阅.
* NSMutiableString在作为属性时使用strong修饰.NSString使用copy修饰,NSMutiableString如果使用copy会发生错误,应为执行set方法会使得ms这个成员变量指向一个不可变对象.
* 关于深浅拷贝: 我们知道调用copy会生成一个不可变对象,调用mutiableCopy会生成一个不可变对象,所谓的深浅拷贝的区别就是**会不会产生新的对象**,[不可变对象 copy] 不会生成新对象所以是浅拷贝, [可变对象 copy]会生成新对象->深拷贝,只要调用mutiableCopy都会产生新对象-> 深拷贝.
注意点:NSMutiableArray对象(简称ma),对ma执行copy操作,会生成一个新的不可变对象NSArray对象(简称a),但是ma中的**元素**并不会执行copy操作,而只是将ma集合中的指针直接赋值给新产生的对象
* kvc 妙用:根据数组中模型的某个属性计算最大,最小,平均值

```objc
NSMutableArray *ma = [NSMutableArray array];
    for (int i = 0; i < 5; i ++) {
        Person *p = [Person new];
        p.age = arc4random_uniform(10);
        NSLog(@"%f",p.age);
        [ma addObject:p];
    }
    NSNumber *ageM = [ma valueForKeyPath:@"@max.age"];
    //@min(最小),@avg(平均值)
```
##### iOS中的事件传递机制:
* 当app接收到触摸事件后将该事件提交到由UIApplication对象管理的事件队列中,然后由application对象负责分发,大概流程为:eventQueue->RootWindow->window.rootViewController->Vc.view->subViews.
其中当事件传递给view时会首先调用该view的`- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event`方法来寻找处理该事件最合适的view,该方法的默认实现大概是以下几个步骤:
> 
 - 检查自身的`userInteractionEnabled`是否为yes
 - 检查自身的`alpha`值是否>0.01
 - 调用`- (BOOL)pointInside:(CGPoint)point withEvent:(UIEvent *)event`来判断事件的触摸点是否在自身的bounds范围内
 
* 以上三点但凡有一个不成立那么该方法会返回nil,那么说明在该view上没有找到能够处理该事件的view,那么该view上所有子view都不会接受该点击事件,子view的`- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event`都不会被调用
* 如果都成立说明该view本身具有接受该事件的能力,该方法会顺着view的层级结构依次调用子view的`- (UIView *)hitTest:(CGPoint)point withEvent:(UIEvent *)event`方法来寻找最合适的view,原则还是上面那三点.找到以后进行返回,如果没有找到那么返回该view本身.

#####获取磁盘空间
```objc
static int64_t _YYDiskSpaceFree() {
    NSError *error = nil;
    NSDictionary *attrs = [[NSFileManager defaultManager] attributesOfFileSystemForPath:NSHomeDirectory() error:&error];
    if (error) return -1;
    //NSFileSystemSize 获取全部磁盘空间
    int64_t space =  [[attrs objectForKey:NSFileSystemFreeSize] longLongValue];
    if (space < 0) space = -1;
    return space;
}
```

#####利用GCD信号量来进行加锁
```objc
//创建锁
_globalInstancesLock = dispatch_semaphore_create(1);
//加锁
dispatch_semaphore_wait(_globalInstancesLock, DISPATCH_TIME_FOREVER);
//do something safely
//解锁
dispatch_semaphore_signal(_globalInstancesLock);
```

#####NSTimer的不安全因素
* iOS开发中实现定时器可以有三种方案:
    - `preferSelfperformSelector: withObject: afterDelay:    `
NSTimer 大概有两类方法可以出发一个定时器进行重复或者定时操作,其中