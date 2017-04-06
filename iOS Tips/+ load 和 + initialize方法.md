###+ Load 和 +initalize 方法
有时候我们希望类先执行某些一次性的初始化操作再使用,NSObject根类中有两个可以实现这种初始化操作的方法,这就是`+Load`和`+initailze`方法
####+Load
#####调用时机
对于加入运行期系统的每个类以及它的分类来说,必定会调用此方法,而且只会被调用一次,通常是在应用程序启动的时候,执行时机在main函数之前!并且先调用父类的+load再调用子类的.

```c
@implementation FatherClass
+(void)load {
    NSLog(@"%s",__func__);
}
@end

@interface SonClass : FatherClass
@end
@implementation SonClass
+(void)load {
    NSLog(@"%s",__func__);
}
@end
//输出台:
+[FatherClass load]
+[SonClass load]
```
如果两个没有继承关系的类都实现了+load方法,那么它的调用顺序取决于谁先被加到运行期环境中
![](/assets/屏幕快照 2017-04-06 上午9.13.34.png)
上图中的AnyObject类与FatherClass类都继承自NSObject,但是FatherClass先被加入进运行期环境,所以它的+load方法会先被执行.
```c
输出台:
+[FatherClass load]
+[SonClass load]
+[AnyObject load]
```
#####使用注意点:
* 在+load的调用时机,系统还处于"脆弱"状态,虽然系统的库已经被加载进运行期系统,但是我们自己编写的类,或者引用的其他的类库中的类不一定已经可以使用,所以在+load中要尽量避免初始化其他的对象. 比如下面的代码就是不安全的
```c
@implementation FatherClass
+(void)load {
    NSLog(@"%s",__func__);
    AnyObject *anyObject = [AnyObject new];
//    use anyObject...
}
@end
```
当然AnyObject这个类使我们自己写的,我们可能通过Complie Sources知道它加载的顺序(这不是一个好办法),但是是用其他类库我们就不得而知,如果在