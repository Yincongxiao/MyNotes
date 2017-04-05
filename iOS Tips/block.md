####Block
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
![](/assets/屏幕快照 2017-04-05 下午9.57.55.png)