
###AFSSLPinningMode
  `AFSecurityPolicy`通过安全连接评估服务器对固定的X.509证书和公钥的信任。

  将固定的SSL证书添加到您的应用程序有助于防止中间人攻击和其他漏洞。 强烈建议处理敏感客户数据或财务信息的应用程序通过HTTPS连接路由所有通信，并配置并启用SSL固定。


	typedef NS_ENUM(NSUInteger, AFSSLPinningMode) {
	    AFSSLPinningModeNone,
	    AFSSLPinningModePublicKey,
	    AFSSLPinningModeCertificate,
	};

