# ReactiveCocoa

### 前言

> 在 **iOS** 编程中我们需要处理各种事件,例如响应按钮的点击,监听键盘的输入,监听网络回包等...我们通常使用cocoa推荐的例如action、delegate、kvo、callback等。
> **ReactiveCocoa**为我们提供了一种统一化的解决此类问题的方式,使用RAC解决问题，就不需要考虑调用顺序，直接考虑结果，把每一次操作都写成一系列嵌套的方法中，使代码高聚合，方便管理。
> reactivecocoa将所有cocoa中的事件都定义为了信号\(single\)，从而可以使用一些基本工具来更容易的连接、过滤和组合.

---

####RAC框架的结构
> 在RAC众多类中最重要的类是**RACSingle**(信号类)

* 它本身不具备发送信号的能力，而是交给内部一个订阅者(id<RACSubscriber>)去发出。
* 默认一个信号都是冷信号，也就是值改变了，也不会触发，只有订阅了这个信号，这个信号才会变为热信号，值改变了才会触发

![RAC类结构图](/assets/ReactiveCocoa v2.5.png)

#### RAC中涉及到的编程思想:

**函数式编程**（functional programming）：使用高阶函数，例如函数用其他函数作为参数。

**响应式编程**（reactive programming）：关注于数据流和变化传播。不需要考虑事件的调用过程,只需要关注数据的流入和输出.

_所以，你可能听说过reactivecocoa被描述为函数响应式编程\(__[FRP](https://en.wikipedia.org/wiki/Functional_reactive_programming)__）框架。_

**链式编程** : 是将多个操作（多行代码）通过点号\(.\)链接在一起成为一句代码,使代码可读性好。a\(1\).b\(2\).c\(3\)

---

### RAC的具体使用

> RACSignal的每个操作都会返回一个RACsignal，这在术语上叫做连贯接口（fluent interface）,从而实现 `链式编程`

#### RAC 核心方法绑定`bind`

对比之前的开发方式是赋值，而用RAC开发，应该把重心放在绑定，也就是在创建一个对象的时候，就绑定好以后想要做的事情，而不是等赋值之后在去做事情。
列如：把数据展示到控件上，之前都是重写控件的setModel方法，用RAC就可以在一开始创建控件的时候，就绑定好数据。

> **注意:**在开发中很少使用`bind`方法，`bind`属于RAC中的底层方法，RAC已经封装了很多好用的其他方法，底层都是调用bind，用法比bind简单.

下面例子中要实现当 _testField_ 的文字改变以后每次输出"输出:\*\*",下面的信号订阅方法底层会转换成bind方法

```objc
//使用 bind 方法
[[_textField.rac_textSignal bind:^RACStreamBindBlock{
        return ^RACStream *(id value, BOOL *stop){
            return [RACReturnSignal return:[NSString stringWithFormat:@"输出:%@",value]];
        };

    }] subscribeNext:^(id x) {
        NSLog(@"%@",x);
    }];
```

```
//直接订阅信号
[_textField.rac_textSignal subscribeNext:^(id x) { NSLog(@"输出:%@",x); }];

```

#### fliter : 过滤信号

> 通过返回**bool**的方式控制信号的传递方式
> 本例中要实现当用户名长度大于3的时候输出log

```objc
[[self.usernameTextField.rac_textSignal
     filter:^BOOL(id value){
        NSString*text = value;
        return text.length > 3;
}] subscribeNext:^(id x){
        NSLog(@"%@", x);
  }];
```

#### Map:\(映射\) map能够"加工信号传递的值”

> 这里的加工信号指的是将上一个next事件传递过来的信号值x进行**加工**或者**转换**成任意的OC对象,通过block返回值返回回去,这个例子中就是将用户名加工成NSNumber值返回回去,所以下一个next事件接收到的值就是NSNumber对象.

```objc
[[[self.usernameTextField.rac_textSignal
  map:^id(NSString*text){
    return @(text.length);
  }]
  filter:^BOOL(NSNumber*length){
    return[length integerValue] > 3;
  }]
  subscribeNext:^(id x){
    NSLog(@"%@", x):
  }];
```

####flatten map: 信号的映射
> flatten可以对传递过来的信号进行加工,会重新新建一个信号,block中的返回值是一个信号


```
 [[_textField.rac_textSignal flattenMap:^RACStream *(id value) {
     return [RACReturnSignal return:[NSString stringWithFormat:@"输出:%@",value]]; }]
              subscribeNext:^(id x) {
}];

```
flattenMap 和 Map 方法的区别:
* flattenMap方法中的block返回只是信号,所以是对信号的加工
* Map 方法中的block的返回值是对象,是对信号传递的变量的加工
* 一般如果传递的是对象,使用map,信号传递的是信号就用flattenMap


#### Reduc聚合: 将多个信号发出的值进行聚合

常用用法:常使用

```
+ (RACSignal *)combineLatest:(id<NSFastEnumeration>)signals reduce:(id (^)())reduceBlock;
```

将信号进行聚合,在reduceBlock中将信号传递的值进行整合.这个方法会创建一个新的信号携带整合好的值返回

```

RACSignal *signalA = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {

   [subscriber sendNext:@1];

   return nil;
}];

RACSignal *signalB = [RACSignal createSignal:^RACDisposable *(id<RACSubscriber> subscriber) {

   [subscriber sendNext:@2];

   return nil;
}];

RACSignal *reduceSignal = [RACSignal combineLatest:@[signalA,signalB] reduce:^id(NSNumber *num1 ,NSNumber *num2){

  return [NSString stringWithFormat:@"%@ %@",num1,num2];

}];

[reduceSignal subscribeNext:^(id x) {

   NSLog(@"%@",x);
}];
```

