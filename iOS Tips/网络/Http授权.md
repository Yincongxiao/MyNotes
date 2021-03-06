####HTTP请求Challenge
在iOS开发中`Challenge`是HTTP中带授权要求的处理机制。
1. 有些URL访问需要具有权限否则返回401的错误，因此客户端需要在HTTP的请求头中带上授权的用户和密码；
2. 当我们使用HTTPS协议时，一旦服务器证书不具备信任则需要客户端确认是否信任此服务器证书；
3. 使用HTTPS协议,当服务端也需要客户端提供证书时；
4. 我们是通过代理服务器来请求HTTP的,我们需要提供代理服务器的用户和密码;
我们把以上这些情况称为服务端要求客户端接收挑战。
当收到服务器发来的以上挑战时,我们客户端会收到一个相应的回调来通知我们,在使用NSURLSession对应的挑战方法是在`NSURLSessionTaskDelegate`中的
```
//session 级别的挑战
- (void)URLSession:(NSURLSession *)session didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge
                                             completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler;
```

```
//特定task的挑战
- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task
                            didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge 
                              completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler;
                              
```
方法,这个方法会在接收到服务器响应的didReceiveResponse之前被调用,旨在让我们根据不同的情况进行处理.
   
#####NSURLAuthenticationChallenge                                                      
`NSURLAuthenticationChallenge`类，这个类是认证挑战类，也就是要求客户端进行挑战，要接收挑战也就是客户端提供挑战的凭证(用户和密码，或者客户端证书，或者信任服务器证书，或者代理），iOS提供了一个NSURLCredential的类来表示凭证,可以通过如下函数来建立挑战凭证.

#####NSURLCredential
这个类代表了授权证书.它的构造方法分别放在三个分类中.
```
@interface NSURLCredential(NSInternetPassword)
//使用用户名和密码建立凭证,用于401错误的挑战凭证和代理的挑战凭证,这种最常见的方式.
- (instancetype)initWithUser:(NSString *)user password:(NSString *)password persistence:(NSURLCredentialPersistence)persistence;
```
```
@interface NSURLCredential(NSClientCertificate)
//在与服务器进行安全握手期间,服务器可能会要求客户端提供安全证书,安全证书一般保存在keychain中.下面的方法就是keychain中取出对应的证书.
- (instancetype)initWithIdentity:(SecIdentityRef)identity certificates:(nullable NSArray *)certArray persistence:(NSURLCredentialPersistence)persistence NS_AVAILABLE(10_6, 3_0);
```
```
@interface NSURLCredential(NSServerTrust)
//这种要求客户端的对服务器的信任来建立凭证，所谓SecTrust用来描述信任某个证书用来做什么的东西，比如一个证书可以用来做SSL，用来做签名，邮件安全（这个证书以及可以用来做什么来构造一个信任）
- (instancetype)initWithTrust:(SecTrustRef)trust NS_AVAILABLE(10_6, 3_0);
```
注意一下以上构造器中的persistence参数,它是一个枚举值,指定把credential保存多久
```c
typedef NS_ENUM(NSUInteger, NSURLCredentialPersistence) {
    NSURLCredentialPersistenceNone,//不保存credential
    NSURLCredentialPersistenceForSession,//只保存在当前回话中
    NSURLCredentialPersistencePermanent,//永久保存,保存在钥匙串中,在OSX中如果用户允许的话是可以访问到所有app保存的credential,但是在iOS中只可以访问到本app的credential.
    NSURLCredentialPersistenceSynchronizable NS_ENUM_AVAILABLE(10_8, 6_0)//永久保存,会被同步到iColud中,在同一个appId下不同设备都可访问
};
```
也就是说我们可以将credential通过以上策略保存起来,而用来保存凭证的类叫做`NSURLCredentialStorage`
#####NSURLCredentialStorage
它是一个单例类,用来管理我们存储的所有credential
#####NSURLProtectionSpace
以上我们知道了服务器向客户端发起挑战,并且挑战分为以上四种类型,那么我们怎么知道服务器给我们的挑战是什么类型的呢,这就用到了NSURLProtectionSpace,(保护空间)
它有以下属性
```c
//只能用在http请求中的401认证,realm值(认证域)
@property (nullable, readonly, copy) NSString *realm;
//认证保护空间是否来自代理服务器
@property (readonly) BOOL isProxy;
//服务端主机地址，如果是代理则代理服务器地址
@property (readonly, copy) NSString *host;
//服务端端口地址，如果是代理则代理服务器的端口
@property (readonly) NSInteger port;
//代理类型，只对代理授权，比如http代理，socket代理等
@property (nullable, readonly, copy) NSString *proxyType;
//代理使用的协议,只对代理授权,如果不是代理,那么为nil
@property (nullable, readonly, copy) NSString *protocol;
```