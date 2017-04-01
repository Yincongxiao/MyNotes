###URL Session生命周期
你可以用两种方式来使用NSURLSession.一种是使用系统自带的delegate,另一种是自定义delegate.如果你有如下的使用场景其中的一个,那么你必须使用自定义的delegate
 * Uses background sessions to download or upload content while your app is not running.
 启动后台任务进行资源的下载或者上传
 * Performs custom authentication.自定义授权
 * Performs custom SSL certificate verification.使用ssl证书验证
 * Decides whether a transfer should be downloaded to disk or displayed based on the MIME type returned by the server or other similar criteria.更改文件存储地址(默认cache)
 * Uploads data from a body stream (as opposed to an NSData object).通过http body上传数据
 * Limits caching programmatically.限制缓存策略
 * Limits HTTP redirects programmatically.限制url重定向
 
 
 #####使用系统提供的delegate
 如果你在创建NSURLSession的时候没有指定delegate对象,那么默认使用系统自带的delegate,下面是一些在使用系统自带的delegate过程中必须实现的步骤
  1 创建一个session configration简称cs对象,如果是后台session这个cs对象必须包含一个唯一的标识,保存这个标识,以后如果app发生crash或者session被暂停或终结以后用来恢复dession数据.
  2 创建session对象,指定上一步中的cs,将sessionDelegate设置为nil
  3 使用session创建session task简称st对象,每一个st对象代表了一次网络请求任务,默认task被创建出来以后是停止状态,需要我们手动调用`- resume`方法开启任务,task一定是` NSURLSessionTask`的子类.分别有`NSURLSessionDataTask, NSURLSessionUploadTask, or NSURLSessionDownloadTask, `,其中`NSURLSessionDataTask`,`NSURLSessionUploadTask`并没有在父类上做扩展,只是用于区别不用的请求任务.
  4 对于下载的task,在服务器传输过程中,我们可以通过` cancelByProducingResumeData:`方法暂停下载任务, 然后通过` downloadTaskWithResumeData: or downloadTaskWithResumeData:completionHandler: `来恢复下载任务
  5 任务完成后就会调用task的`completion handler`
  
  ```c
  NSURL *url = [NSURL URLWithString:@"http://example.com/path/index.json"];
   //在request这里我们可以对此次请求进行一些设置,比如cachePolicy,timeoutInterval等
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url cachePolicy:NSURLRequestUseProtocolCachePolicy timeoutInterval:0.5];
    //此方法在NSMutableURLRequest (NSMutableHTTPURLRequest)中声明.
    //所以只有NSMutableURLRequest才能设置请求方法,请求体中可以设置cookie等信息
    request.HTTPMethod = @"GET";
    //如果completionHandler设置为nil,那你应该给session设置delegate,在代理方法中接受网络回调
    //data代表服务器返回的数据,response包含了请求的url和回包的header等信息.
    NSURLSessionDataTask *dataTask = [_session dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        if (error) {
            NSLog(@"%@",error.userInfo[NSLocalizedDescriptionKey]);
        }else {
            NSError *e;
            NSDictionary *jsonDic = [NSJSONSerialization JSONObjectWithData:data options:NSJSONReadingMutableContainers error:&e];
            NSLog(@"request success%@!",jsonDic);
        }
    }];
    //默认task是暂停的,要手动resume
    [dataTask resume];
  ```
 #####使用自定义的delegate
 
