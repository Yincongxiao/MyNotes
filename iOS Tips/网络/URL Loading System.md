####iOS网络请求相关知识
#####URL Loading System
> 说到iOS中的网络首先要从URL Loading System开始说起

URL Loading System (简称ULS)允许我们使用url来获取或者操作资源.这个系统的设计核心是`NSURL`类,它是由一系列类和协议组成,这些类可以帮助我们获取资源,向服务器上传文件,管理cookie的存取,控制网络回包的缓存,自定义证书和验证,编写我们自己的协议扩展.
ULS支持以下协议来获取资源.

ULS的结构![](/assets/URL Loading System.png)

 * File Transfer Protocol (ftp://)
 * Hypertext Transfer Protocol (http://)
 * Hypertext Transfer Protocol with  
 encryption (https://)
 * Local file URLs (file:///)
 * Data URLs (data://)
 
 