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
