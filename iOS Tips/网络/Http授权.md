####HTTP请求要求授权
在iOS开发中
1. HTTP中带授权要求的处理机制。有些URL访问需要具有权限否则返回401的错误，因此客户端需要在HTTP的请求头中带上授权的用户和密码；
2. 或者当我们使用HTTPS协议时，一旦服务器证书不具备信任则需要客户端确认是否信任此服务器证书；或者用HTTPS协3. 议当服务端也需要客户端提供证书时；
4. 我们是通过代理服务器来请求HTTP的,我们需要提供代理服务器的用户和密码，我们称这些情况称为服务端要求客户端接收挑战。
当收到服务器发来的以上挑战时,我们客户端会受到一个相应的回调来通知我们,在使用NSURLSession对应的挑战方法是在`NSURLSessionTaskDelegate`中的
```
- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task
                            didReceiveChallenge:(NSURLAuthenticationChallenge *)challenge 
                              completionHandler:(void (^)(NSURLSessionAuthChallengeDisposition disposition, NSURLCredential * _Nullable credential))completionHandler;
                              
```
                              
`NSURLAuthenticationChallenge`类，这个类是认证挑战类，也就是要求客户端进行挑战，要接收挑战也就是客户端提供挑战的凭证(用户和密码，或者客户端证书，或者信任服务器证书，或者代理），iOS提供了一个NSURLCredential的类来表示挑战凭证。