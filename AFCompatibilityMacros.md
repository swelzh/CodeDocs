##AFCompatibilityMacros

AFCompatibilityMacros其实是一个比较简单的文件

	#ifdef API_UNAVAILABLE
    #define AF_API_UNAVAILABLE(x) API_UNAVAILABLE(x)
	#else
    #define AF_API_UNAVAILABLE(x)  //不做任何事情
	#endif // API_UNAVAILABLE
	



