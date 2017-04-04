iCloud Document Thumbnails###NSURL
####URL
> URL统一资源定位符是对可以从互联网上得到的资源的位置和访问方法的一种简洁的表示，是互联网上标准资源的地址。互联网上的每个文件都有一个唯一的URL，它包含的信息指出文件的位置以及浏览器应该怎么处理它。

#####URL结构
完整的、带有授权部分的普通统一资源标志符语法看上去如下：协议://用户名:密码@子域名.域名.顶级域名:端口号/目录/文件名.文件后缀?参数=值#标志
```
                hierarchical part
        ┌───────────────────┴─────────────────────┐
                    authority               path
        ┌───────────────┴───────────────┐┌───┴────┐
  abc://username:password@example.com:123/path/data?key=value&key2=value2#fragid1
  └┬┘   └───────┬───────┘ └────┬────┘ └┬┘           └─────────┬─────────┘ └──┬──┘
scheme  user information     host     port                  query         fragment

  urn:example:mammal:monotreme:echidna
  └┬┘ └────────────┬───────────────┘
scheme              path
```
#####绝对URL与相对URL
 * **绝对URL**
绝对URL（absolute URL）显示文件的完整路径，这意味着绝对URL本身所在的位置与被引用的实际文件的位置无关，
 * **相对URL**
相对URL（relative URL）以包含URL本身的文件夹的位置为参考点，描述目标文件夹的位置。如果目标文件与当前页面（也就是包含URL的页面）在同一个目录，那么这个文件的相对URL仅仅是文件名和扩展名，如果目标文件在当前目录的子目录中，那么它的相对URL是子目录名，后面是斜杠，然后是目标文件的文件名和扩展名。
如果要引用文件层次结构中更高层目录中的文件，那么使用两个句点和一条斜杠。可以组合和重复使用两个句点和一条斜杠，从而引用当前文件所在的硬盘上的任何文件，
一般来说，对于同一服务器上的文件，应该总是使用相对URL，它们更容易输入，而且在将页面从本地系统转移到服务器上时更方便，只要每个文件的相对位置保持不变，链接就仍然是有效地。

####NSURL
一个NSURL代表了一个资源的url,资源可以是放在远程服务器上的,也可以是放在本地磁盘的,或者是内存中的二进制数据.
* 通过NSURL,你可以直接访问资源文件,如果这个url代表这本地文件,你甚至可以改变这个文件的最后修改时间.你还可以使用NSURLConnection,NSURLSession,NSURLDownload等技术来进行远程服务器的访问.详细可以看[URL Session Programming Guide.
](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/URLLoadingSystem/URLLoadingSystem.html#//apple_ref/doc/uid/10000165i)

* 苹果推荐我们使用URL的方式进行本地文件的访问,大部分对本地文件的读写方法中都包含了通过NSURL的方式,例如你可以通过`init​With​Contents​Of​URL:​encoding:​error:​ `来初始化NSString类型的资源,或者`init​With​Contents​Of​URL:​options:​error:​ `来初始化NSData类型的资源.
* 你可以通过url来进行app之间的沟通,例如在macOS中NSWorkspace提供了`openURL:`方法,与之对应的iOS中UIApplication类也提供了`openURL:`方法.
* 在appKit framework中可以通过url进行剪切板的使用.
* NSURL类与CoreFundation中的`CFURLRef`是Toll-Free Bridging 的.

#####NSURL的结构
一个NSURL对象由两部分组成:一个可能为nil的baseURL和一个相对于baseURL的path部分.如果一个NSURL对象的baseURL为nil,那么我们认为path部分就是url的绝对地址.
```c
NSURL *baseURL = [NSURL fileURLWithPath:@"file:///path/to/user/"];
NSURL *URL = [NSURL URLWithString:@"folder/file.html" relativeToURL:baseURL];
NSLog(@"absoluteURL = %@", [URL absoluteURL]);///file:​///path/to/user/folder/file.html.
```
我们可以使用NSURL属性来获取到url中的各个部分.例如url` https:​//johnny:​p4ssw0rd@www.example.com:​443/script.ext;param=value?query=value#ref`可以分解为以下几部分.

Component | Value
-|-
scheme|https
user|johnny
password|p4ssw0rd
host|www.example.com
port|443
path|/script.ext
path​Extension|ext
path​Components|["/", "script.ext"]
parameter​String|param=value
query|query=value
fragment|ref

#####BookMark and Security Scope
######bookmark
bookMarks是从iOS4.0以后NSURL提供的一种引用本地文件系统资源的方式.通过bookmark我们可以获得本地文件的引用,如果用户重启我们app,或者用户将该资源移动到其他文件夹,或者对该文件重命名,我们仍然可以使用bookmark来获取到该资源.
根据文件的URL创建bookmark
```c
- (NSData*)bookmarkForURL:(NSURL*)url {
    NSError* theError = nil;
    NSData* bookmark = [url bookmarkDataWithOptions:NSURLBookmarkCreationSuitableForBookmarkFile
                                            includingResourceValuesForKeys:nil
                                            relativeToURL:nil
                                            error:&theError];
    if (theError || (bookmark == nil)) {
        // Handle any errors.
        return nil;
    }
    return bookmark;
}
```
读取bookmark

```c
- (NSURL*)urlForBookmark:(NSData*)bookmark {
    BOOL bookmarkIsStale = NO;
    NSError* theError = nil;
    NSURL* bookmarkURL = [NSURL URLByResolvingBookmarkData:bookmark
                                            options:NSURLBookmarkResolutionWithoutUI
                                            relativeToURL:nil
                                            bookmarkDataIsStale:&bookmarkIsStale
                                            error:&theError];
 
    if (bookmarkIsStale || (theError != nil)) {
        // Handle any errors
        return nil;
    }
    return bookmarkURL;
}
```
######Security-Scope URLs
Security-scoped URLs提供了获取app沙盒之外的资源的方法,在iOSapp中使用[ UIDocument​Picker​View​Controller](https://developer.apple.com/reference/uikit/uidocumentpickerviewcontroller?language=objc)来打开或者移除documents的时候需要用到Security-scoped URLs
对于iOSapp,如果你是用[UIDocument](https://developer.apple.com/reference/uikit/uidocument?language=objc)去获取url,那么系统会帮我们管理security-scoped URL
如果使用Security-scoped URLs来获取资源会用到NSURL的`-start​Accessing​Security​Scoped​Resource`方法(或者使用Core Fundation中的`CFURLStop​Accessing​Security​Scoped​Resource `函数).在使用完毕后一定要使用`- stop​Accessing​Security​Scoped​Resource `(或者Core Fundation的`CFURLStop​Accessing​Security​Scoped​Resource`函数)注销这个请求
* 假如注销失败(返回值为NO)会造成内存泄露,如果内存泄露到一定程度那么你的app将失去通过file -system locations访问沙盒,Powerbox,security-scoped bookmarks,的权限!

######iCloud Document Thumbnails
iOS8.0之后NSURL包含了直接读取和设置document thumbnails的方式来改变iCloud document

```c
NSURL *URL = [self URLForDocument];
NSDictionary *thumbnails = nil;
NSError *error = nil;
 
BOOL success = [URL getPromisedItemResourceValue:&thumbnails
                                          forKey:NSURLThumbnailDictionaryKey
                                           error:&error];
if (success) {
  NSImage *image = thumbnails[NSThumbnail1024x1024SizeKey];
} else {
  // handle the error
}
```




