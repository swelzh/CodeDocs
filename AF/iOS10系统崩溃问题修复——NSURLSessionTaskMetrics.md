#[iOS10系统崩溃问题修复——NSURLSessionTaskMetrics](https://www.unko.cn/2017/07/10/iOS10%E7%B3%BB%E7%BB%9F%E5%B4%A9%E6%BA%83%E9%97%AE%E9%A2%98%E4%BF%AE%E5%A4%8D%E2%80%94%E2%80%94NSURLSessionTaskMetrics/)


###现象
奔溃日志：

	CoreFoundation    0x1816721d8    ___exceptionPreprocess + 148
	libobjc.A.dylib    0x1800ac55c    _objc_exception_throw
	Foundation    0x182280b4c    -[_NSConcreteDateInterval dealloc] + 0
	CFNetwork    0x181eab3e0    -[__NSCFURLSessionTaskMetrics _initWithTask:] + 692
	CFNetwork    0x181eab028    -[NSURLSessionTaskMetrics _initWithTask:] + 100
	CFNetwork    0x181d3016c    -[__NSCFURLLocalSessionConnection _tick_finishing] + 372
	libdispatch.dylib    0x1804fd200    __dispatch_call_block_and_release + 24
	libdispatch.dylib    0x1804fd1c0    __dispatch_client_callout + 16
	libdispatch.dylib    0x18050b444    __dispatch_queue_serial_drain + 928
	libdispatch.dylib    0x1805009a8    __dispatch_queue_invoke + 652
	libdispatch.dylib    0x18050d38c    __dispatch_root_queue_drain + 572
	libdispatch.dylib    0x18050d0ec    __dispatch_worker_thread3 + 124
	libsystem_pthread.dylib    0x1807062b8    __pthread_wqthread + 1288
	libsystem_pthread.dylib    0x180705da4    _start_wqthread + 4
	

NSURLSessionTaskMetrics这个类在 iOS10.0新增。此崩溃发生在iOS10.0 - 10.2，不包括 iOS10.2。

在[Open Radar](https://openradar.appspot.com/28301343)上找到了这个 bug report。

	NSGenericException "Start date cannot be later in time than end date!" in -[NSURLSessionTaskMetrics _initWithTask:]
	Originator:	nobrien	
	Number:	rdar://28301343	Date Originated:	14-Sep-2016 09:16 AM
	Status:	Open	Resolved:	
	Product:	iOS SDK	Product Version:	10
	Classification:	Crash/Hang/Data Loss	Reproducible:	Unable
	 
	Summary:
	Trickling in crashes with NSGenericException - Start date cannot be later in time than end date!
	Steps to Reproduce:
	Cannot reproduce locally
	Expected Results:
	No exception
	Actual Results:
	Exception
	Regression:
	New to iOS 10 (makes sense since this is in the new NSURLSessionTaskMetrics code
	Notes:
	Is there potential risk with clock skew?  FWIW: we do all our timing metrics with mach time to avoid potential issues with timezone traversals, regional settings changes or daylight savings time changes.
	
	
	