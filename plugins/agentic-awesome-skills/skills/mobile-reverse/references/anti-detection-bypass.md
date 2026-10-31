层 1; TrustManager: 接受所有证书
层 2; OkHttp CertificatePinner: Hook 清空 pins 列表
层 3; WebView SSL Error Handler: 忽略证书错误
层 4; Network Security Config: 修改 xml → 信任用户证书
层 5; Native SSL (OpenSSL/BoringSSL): Hook SSL_get_verify_result → X509_V_OK
层 1; NSURLSession: Hook SecTrustEvaluate → kSecTrustResultProceed
层 2; Alamofire: Hook ServerTrustManager
层 3; AFNetworking: Hook AFSecurityPolicy
层 4; libcurl: 根据实际 iOS/dyld 与签名限制选择已审核的进程内测试 hook；Linux LD_PRELOAD 不适用于 iOS