```
NSError *__autoreleasing error; 
￼if (![data writeToFile:filename options:NSDataWritingAtomic error:&error]) 
￼{ 
　　NSLog(@"Error: %@", error); 
}
```
（在上面的writeToFile方法中error参数的类型为(NSError *__autoreleasing *)）

注意，如果你的error定义为了strong型，那么，编译器会帮你隐式地做如下事情，保证最终传入函数的参数依然是个__autoreleasing类型的引用。

```
NSError *error; 
NSError *__autoreleasing tempError = error; // 编译器添加 
if (![data writeToFile:filename options:NSDataWritingAtomic error:&tempError]) 
￼{ 
　　error = tempError; // 编译器添加 
　　NSLog(@"Error: %@", error); 
}
```
所以为了提高效率，避免这种情况，我们一般在定义error的时候将其（老老实实地=。=）声明为__autoreleasing类型的：
```
NSError *__autoreleasing error;

```
在这里，加上__autoreleasing之后，相当于在MRC中对返回值error做了如下事情
```
*error = [[[NSError alloc] init] autorelease];
```
*error指向的对象在创建出来后，被放入到了autoreleasing pool中，等待使用结束后的自动释放，函数外error的使用者并不需要关心*error指向对象的释放。

另外一点，在ARC中，所有这种指针的指针 （NSError **）的函数参数如果不加修饰符，编译器会默认将他们认定为__autoreleasing类型。比如下面的两段代码是等同的：
```
- (NSString *)doSomething:(NSNumber **)value
{
        // do something  
}
- (NSString *)doSomething:(NSNumber * __autoreleasing *)value
{
        // do something  
}
//除非你显式得给value声明了__strong，否则value默认就是__autoreleasing的。
```
* 最后一点，某些类的方法会隐式地使用自己的autorelease pool，在这种时候使用__autoreleasing类型要特别小心。
比如NSDictionary的[enumerateKeysAndObjectsUsingBlock]方法：
```- (void)loopThroughDictionary:(NSDictionary *)dict error:(NSError **)error
{
    [dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop){

          // do stuff  
          if (there is some error && error != nil)
          {
                *error = [NSError errorWithDomain:@"MyError" ￼code:1 userInfo:nil];
          }
￼
    }];
￼}
```
会隐式地创建一个autorelease pool，上面代码实际类似于：
```
- (void)loopThroughDictionary:(NSDictionary *)dict error:(NSError **)error
{
    [dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop){

          @autoreleasepool  // 被隐式创建
　　　　　　{
              if (there is some error && error != nil)
              {
                    *error = [NSError errorWithDomain:@"MyError" ￼code:1 userInfo:nil];
              }
￼          }
    }];

    // *error 在这里已经被dict的做枚举遍历时创建的autorelease pool释放掉了 ：(  
￼}    

```
为了能够正常的使用*error，我们需要一个strong型的临时引用，在dict的枚举Block中是用这个临时引用，保证引用指向的对象不会在出了dict的枚举Block后被释放，正确的方式如下：
```
- (void)loopThroughDictionary:(NSDictionary *)dict error:(NSError **)error
{
　　__block NSError* tempError; // 加__block保证可以在Block内被修改  
　　[dict enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop)
　　{ 
　　　　if (there is some error) 
　　　　{ 
　　　　　　*tempError = [NSError errorWithDomain:@"MyError" ￼code:1 userInfo:nil]; 
　　　　} ￼ 

　　}] 

　　if (error != nil) 
　　{ 
　　　　*error = tempError; 
　　} ￼
} 
```