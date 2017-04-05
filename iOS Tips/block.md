####Block
#####简单介绍
Block可以实现闭包,作为扩展加入GCC编译器中,10.4中版本及其后的Mac OS X系统与iOS4.0以后都含有正常执行块所需的运行期组件.
block与函数类似,只不过是定义在另一个函数中,与定义block的函数共享同一范围内的东西,block其实就是个值,有自己的数据类型,与int,float,oc对象一样,也可以把block赋给变量,然后像使用其他变量那样使用block,下面是简单的定义一个block并使用它.
```c
 NSString *foo = @"foo";
    void (^printStringBlock)(NSString *string) = ^(NSString *string) {
        NSLog(@"string need print is %@, and foo is %@", string, foo);
    };
    NSLog(@"just divided line -----");
    printStringBlock(@"hello word!");
    //just divided line -----
    //string need print is hello word!, and foo is foo
```
上面的block代表了没有返回值,接受一个NSString类型的参数的block.
只有当下面调用他的时候,`^{ }`内的代码块才会得到执行.也就有了上面的打印顺序.
要注意的是在生命block的范围内,所有的变量都可以被它捕获,也就是为什么在printStringBlock中能够访问到foo变量.
并且默认情况下被block捕获的变量是不能在block中修改的.不过,声明变量的时候可以加上__block修饰符,这样就可以在block内部修改该变量了
上面的实例展示了先将block赋值给局部变量.下面展示了内链块的用法.
```c
- (void)getNameUsingBlock:(void(^)(NSString *))resultBlock {
    if (resultBlock) {
        resultBlock(@"asnail~");
    }
}
//...using
[self getNameUsingBlock:^(NSString *name) {
        NSLog(@"name is %@", name);
    }];
//name is asnail~
```
如果block所捕获的是对象类型,那么就会自动保留它,系统在释放这个block的时候也会将其一并释放,块和其他oc对象一样也有`引用计数`,当最后一个指向block的引用移走之后,block就回收了,回收时同时也会释放block所捕获的变量,一边平衡捕获时所执行的保留操作.
![](/assets/屏幕快照 2017-04-05 下午9.57.55.png)