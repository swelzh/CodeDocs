



##2.原子操作
狭义上的原子操作表示一条不可打断的操作，也就是说线程在执行操作过程中，不会被操作系统挂起，而是一定会执行完（理论上拥有CPU时间片无限长）。

在单处理器环境下，一条汇编指令显然是原子操作，因为中断也要通过指令来实现，但一句高级语言的代码却不是原子的，因为它最终是由多条汇编语言完成，CPU在进行时间片切换时，大多都会在某条代码的执行过程中。
但在多核处理器下，则需要硬件支持。

##@synchronized
代表这个方法加锁, 相当于不管哪一个线程（例如线程A），运行到这个方法时,都要检查有没有其它线程例如B正在用这个方法，有的话要等正在使用synchronized方法的线程B运行完这个方法后再运行此线程A,没有的话,直接运行。

@synchronized是几种iOS多线程同步机制中最慢的一个，同时也是最方便的一个。

@synchronized还有个很容易变慢的场景，就是{}内部有其他隐蔽的函数调用。比如：
	
	@synchronized (tokenA) {
	    [arrA addObject:obj];
	    [self doSomethingWithA:arrA];
	}

doSomethingWithA内部可能又调用了其他函数，维护doSomethingWithA的工程师可能并没有意识到自己是被锁同步的，由此层层叠叠可能引入更多的函数调用，代码就莫名其妙的越来越慢了，感觉锁的性能差，其实是我们没用好。

###死锁






##AFURLSessionManager
configuration = [NSURLSessionConfiguration defaultSessionConfiguration];


##NSURLSessionConfiguration

defaultSessionConfiguration是全局单例证书, cache并且cookie storage objects.


##Cache和Cookies区别是什么
###什么是cookies？
cookies是一些对请求有用的信息，包括密码、偏好、浏览器、IP地址、时间、访问次数等.
每次网络请求，浏览器/APP会将cookie发给服务器，告诉服务器用户之前的活动

cookies生命周期的时长由创建者定义，时长到期后就会被清除.
###什么是cache？
cache是用来缓存从服务端接受的信息， 如HTML页面、图片、视频等，减少带宽使用，为了更好的浏览体验

###所以区别是：
Cookie和Cache存在不同，是因为他们存在的目的不一样

Cookie用于存储信息以跟踪与用户相关的不同特征，而缓存用于加快网页加载。
 
Cookies存储用户登录信息，偏好等信息，而缓存将保留音频，视频等资源文件。
 
通常，Cookie会在一段时间后过期，但缓存会保留在客户端的计算机中，直到用户手动删除它们为止。





###AFURLSessionManager
AFHTTPSessionManager继承自AFURLSessionManager，设置了baseURL以后，
可以便利地进行网络请求


## Methods to Override
dataTaskWithRequest:uploadProgress:downloadProgress:completionHandler:

## Serialization


##Property

baseURL只有在initWithBaseURL中可以初始化

	(readonly, nullable) NSURL *baseURL;


request数据序列器
	
	AFHTTPRequestSerializer <AFURLRequestSerialization> * requestSerializer;

response数据序列器

	AFHTTPResponseSerializer <AFURLResponseSerialization> * responseSerializer;
	
安全规则
	
	AFSecurityPolicy *securityPolicy;


## API 

生成实例的方法

	+ (instancetype)manager;

初始化方法
	
	- (instancetype)initWithBaseURL:(nullable NSURL *)url;
	- (instancetype)initWithBaseURL:(nullable NSURL *)url
           sessionConfiguration:(nullable NSURLSessionConfiguration *)configuration
           

便利请求方法，用于发起GET/POST/PUT/DELETE请求方法;
同时返回NSURLSessionDataTask.


####请求方法，以POST请求为例
	
	

	- (NSURLSessionDataTask *)POST:(NSString *)URLString
                    parameters:(id)parameters
                       headers:(NSDictionary<NSString *,NSString *> *)headers
     constructingBodyWithBlock:(void (^)(id<AFMultipartFormData> _Nonnull))block
                      progress:(void (^)(NSProgress * _Nonnull))uploadProgress
                       success:(void (^)(NSURLSessionDataTask * _Nonnull, id _Nullable))success failure:(void (^)(NSURLSessionDataTask * _Nullable, NSError * _Nonnull))failure
	{
	 // 
    NSError *serializationError = nil;
    NSMutableURLRequest *request = [self.requestSerializer multipartFormRequestWithMethod:@"POST" URLString:[[NSURL URLWithString:URLString relativeToURL:self.baseURL] absoluteString] parameters:parameters constructingBodyWithBlock:block error:&serializationError];
    for (NSString *headerField in headers.keyEnumerator) {
        [request addValue:headers[headerField] forHTTPHeaderField:headerField];
    }
    if (serializationError) {
        if (failure) {
            dispatch_async(self.completionQueue ?: dispatch_get_main_queue(), ^{
                failure(nil, serializationError);
            });
        }
        
        return nil;
    }
    
    __block NSURLSessionDataTask *task = [self uploadTaskWithStreamedRequest:request progress:uploadProgress completionHandler:^(NSURLResponse * __unused response, id responseObject, NSError *error) {
        if (error) {
            if (failure) {
                failure(task, error);
            }
        } else {
            if (success) {
                success(task, responseObject);
            }
        }
    }];
    
   	 [task resume];
    
     return task;
	}



	
###NSStream
在Cocoa中包含三个与流相关的类：NSStream、NSInputStream和NSOutputStream。NSStream是一个抽象基类，定义了所有流对象的基础接口和属性。NSInputStream和NSOutputStream继承自NSStream，实现了输入流和输出流的默认行为。下图描述了流的应用场景：
 
	
###NSSortDescriptor
[NSSortDescriptor sortDescriptorWithKey:@"description" ascending:YES selector:@selector(compare:)]


###Content-Disposition
####作为multipart body中的消息头节

在multipart/form-data类型的应答消息体中， Content-Disposition 消息头可以被用在multipart消息体的子部分中，用来给出其对应字段的相关信息。各个子部分由在Content-Type 中定义的分隔符分隔。用在消息体自身则无实际意义。

[mutableHeaders setValue:[NSString stringWithFormat:@"form-data; name=\"%@\"", name] forKey:@"Content-Disposition"];

###static inline 

内联函数有些类似于宏。内联函数的代码会被直接嵌入在它被调用的地方，调用几次就嵌入几次，没有使用call指令。这样省去了函数调用时的一些额外开销，比如保存和恢复函数返回地址等，可以加快速度。

NSString * AFMultipartFormInitialBoundary(NSString *boundary) {
    return [NSString stringWithFormat:@"--%@%@", boundary, kAFMultipartFormCRLF];
}




