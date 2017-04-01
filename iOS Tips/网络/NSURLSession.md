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
 如果你在创建NSURLSession的时候没有指定delegate对象,那么默认使用系统自带的delegate,
 #####使用自定义的delegate
 
