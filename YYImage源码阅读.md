




	
##关键类YYImage
YYImage是继承自UIImage，override了UIImage的方法。

额外地：

1、YYImage遵守YYAnimatedImage协议

2、新增3个readonly的animatedImage属性: animatedImageType,  animatedImageData,  animatedImageMemorySize


作为接口来说，YYImage使用比较简洁。

#####API： override UIImage的方法

	+ (nullable YYImage *)imageNamed:(NSString *)name; // no cache!
 
	+ (nullable YYImage *)imageWithContentsOfFile:(NSString *)path;
 
	+ (nullable YYImage *)imageWithData:(NSData *)data;
 
	+ (nullable YYImage *)imageWithData:(NSData *)data scale:(CGFloat)scale;

#####@Property 

	@property (nonatomic, readonly) YYImageType animatedImageType;

	@property (nullable, nonatomic, readonly) NSData *animatedImageData;
	 
	@property (nonatomic, readonly) NSUInteger animatedImageMemorySize;

	@property (nonatomic) BOOL preloadAllAnimatedImageFrames;



###@protocol YYAnimatedImage <NSObject>
	
	@required 
	- (NSUInteger)animatedImageFrameCount; 
	- (NSUInteger)animatedImageLoopCount; 
	- (NSUInteger)animatedImageBytesPerFrame; 
	- (nullable UIImage *)animatedImageFrameAtIndex:(NSUInteger)index;
 
	- (NSTimeInterval)animatedImageDurationAtIndex:(NSUInteger)index;

	@optional
 
	- (CGRect)animatedImageContentsRectAtIndex:(NSUInteger)index;


核心API
+ (YYImage *)imageNamed:(NSString *)name



##### 常见宏定义
判断是否引入了该框架

	#if __has_include(<UIKit/UIKit.h>)
   		 NSLog(@"包含");
	#else
   		 NSLog(@"不包含");
	#endif
	