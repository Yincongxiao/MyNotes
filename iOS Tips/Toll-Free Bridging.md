####Toll-Free Bridging
翻译自[Apple](https://developer.apple.com/library/content/documentation/General/Conceptual/CocoaEncyclopedia/Toll-FreeBridgin/Toll-FreeBridgin.html#//apple_ref/doc/uid/TP40010810-CH2).
我们日常开发中一般使用Fundation的类,其中有一些是跟Core Fundation 框架中的类是可以进行内部转换的,这个特性就被称之为`Toll-Free Bridging`,意味着你可以使用同一种数据结构作为Core Fundation 中函数的参数,或者作为Fundation中方法的参数.例如[NSLocal](https://developer.apple.com/reference/foundation/nslocale)与[CFLocal](https://developer.apple.com/reference/corefoundation/cflocale)就是Toll-Free Bridging 的.所以,如果你在使用一个需要传递(NSLocal *)类型的参数的OC方法的时候,你可以传递一个CFLocalRef类型的值进去.反过来也一样.你可以只创建以一种类型的对象s来避免编译器的警告.
例如以下的例子
```c
NSLocale *gbNSLocale = [[NSLocale alloc] initWithLocaleIdentifier:@"en_GB"];
CFLocaleRef gbCFLocale = (CFLocaleRef) gbNSLocale;
CFStringRef cfIdentifier = CFLocaleGetIdentifier (gbCFLocale);
NSLog(@"cfIdentifier: %@", (NSString *)cfIdentifier);
// logs: "cfIdentifier: en_GB"
CFRelease((CFLocaleRef) gbNSLocale);
 
CFLocaleRef myCFLocale = CFLocaleCopyCurrent();
NSLocale * myNSLocale = (NSLocale *) myCFLocale;
[myNSLocale autorelease];
NSString *nsIdentifier = [myNSLocale localeIdentifier];
CFShow((CFStringRef) [@"nsIdentifier: " stringByAppendingString:nsIdentifier]);
// logs identifier for current locale
```
这里需要注意的是,我们用到的内存管理语句同样是Toll-Free Bridging的,所以你可以使用CFRelease来释放Cocoa对象,也可以吧release和autorelease用在Core Fundation的对象上
但是如果使用garbage collectin 进行内存管理的话,Core Fundation 和Cocoa对象会有很大的差异.
下面有一张对照表,列出了在Core Fundation和在Fundation中是Toll-Free Bridging转换的

| Core Fundation Type| Fundation class | Availability |
| : --------- : | : ------ : |: ------ :|
|CFArrayRef| NSArray| OS X 10.0|
|CFAttributedStringRef| NSAttributedString|OS X 10.4|
|CFBooleanRef|NSNumber|OS X 10.0|
|CFCalendarRef|NSCalendar|OS X 10.4|
|CFCharacterSetRef|NSCharacterSet|OS X 10.0|
|CFDataRef|NSData|OS X 10.0|
|CFDateRef|NSDate|OS X 10.0|
|CFDictionaryRef|NSDictionary|OS X 10.0|
|CFErrorRef|NSError|OS X 10.5|
|CFLocaleRef|NSLocale|OS X 10.4|
|CFMutableArrayRef|NSMutableArray|OS X 10.0|
|CFMutableAttributedStringRef|NSMutableAttributedString|OS X 10.4|
|CFMutableCharacterSetRef|NSMutableCharacterSet|OS X 10.0|
|CFMutableDataRef|NSMutableData|OS X 10.0|
|CFMutableDictionaryRef|NSMutableDictionary|OS X 10.0|
|CFMutableSetRef|NSMutableSet|OS X 10.0|
|CFMutableStringRef|NSMutableString|OS X 10.0|
|CFNullRef|NSNull|OS X 10.2|
|CFNumberRef|NSNumber|OS X 10.0|
|CFReadStreamRef|NSInputStream|OS X 10.0|
|CFRunLoopTimerRef|NSTimer|OS X 10.0|
|CFSetRef|NSSet|OS X 10.0|
|CFStringRef|NSString|OS X 10.0|
|CFTimeZoneRef|NSTimeZone|OS X 10.0|
|CFURLRef|NSURL|OS X 10.0|
|CFWriteStreamRef|NSOutputStream|OS X 10.0|
* 注意:并不是所有的类都是Toll-Free Bridging 的,例如NSRunLoop和CFRunLoopRef就不是,NSDateFormate和CFDateFormateRef也不是.
```c
NSArray *anNSArray = @[@1,@2,@3];
CFArrayRef anCFArray = (__bridge CFArrayRef)anNSArray;
NSLog(@"size of array is %li",CFArrayGetCount(anCFArray));
输出台:
//size of array is = 3
```
这里用到了`__bridge`关键字,它的意思是,虽然将NSArray类型的对象转成了CFArrayRef类型的结构,但是ARC仍然对该对象具有内存管理权限.
```c
NSArray *anNSArray = @[@1,@2,@3];
CFArrayRef anCFArray = (__bridge_retained CFArrayRef)anNSArray;
NSLog(@"size of array is %li",CFArrayGetCount(anCFArray));
CFRelease(anCFArray);
输出台:
//size of array is = 3
```
上面用到了`__bridge_retained`关键字,它的意思是ARC将anNSArray对象的内存管理权限全部交出,所以我们要自己管理该对象的内存.记得最后调用CFRelease(anCFArray);
