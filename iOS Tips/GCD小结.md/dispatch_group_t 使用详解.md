####dispatch_group
> 通常我们执行耗时操作会放到子线程中并发执行,这个过程中我们可能想知道各个任务全部执行完毕的时刻,当然我们可以通过记录一些标志位等手段来达到要求,但是可能步骤会比较复杂.强大的GCD中的`dispatch_group_t`可以轻松帮我们达到目的.

* 我们先来看group中的api
  * dispatch_group_async
  
 ```c
 //将block(任务)提交到指定的队列中,并且将次任务放到(关联)指定的group,block将异步执行.
 void dispatch_group_async(dispatch_group_t group,dispatch_queue_t queue,dispatch_block_t block);		
 //上面方法的函数版本.接受一个函数指针.
void dispatch_group_async_f(dispatch_group_t group,dispatch_queue_t queue,void *_Nullable context,dispatch_function_t work);		
 ```
 * dispatch_group_wait
 ```c
 //这个方法会同步的等,接受一个timeout,group的任务全部完成或者超时以后会返回,返回值为0代表任务完成,非0代表超时.
 long dispatch_group_wait(dispatch_group_t group,dispatch_time_t timeout);
 ```
 
 ```c
 void dispatch_group_notify(dispatch_group_t group,dispatch_queue_t queue,dispatch_block_t block);
 ```