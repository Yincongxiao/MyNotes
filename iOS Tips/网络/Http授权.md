####HTTP请求Challenge
在iOS开发中`Challenge`是HTTP中带授权要求的处理机制。
1. 有些URL访问需要具有权限否则返回401的错误，因此客户端需要在HTTP的请求头中带上授权的用户和密码；
2. 当我们使用HTTPS协议时，一旦服务器证书不具备信任则需要客户端确认是否信任此服务器证书；或者用HTTPS协3. 议当服务端也需要客户端提供证书时；
4. 我们是通过代理服务器来请求HTTP的,我们需要提供代理服务器的用户和密码，我们称这些情况称为服务端要求客户端接收挑战。
当收到服务器发来的以上挑战时,我们客户端会受到一个相应的回调来通知我们,在使用NSURLSession对应的挑战方法是在`NSURLSessionTaskDelegate`中的
```
- (void)URLSession:(NSURLSession *)session didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge
                                             completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler;
```

```
- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task
                            didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge 
                              completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler;
                              
```
方法,这个方法会在接收到服务器响应的didReceiveResponse之前被调用,旨在让我们根据不同的情况进行处理.
                             
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