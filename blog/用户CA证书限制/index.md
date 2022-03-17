# 用户CA证书限制


<!--more-->

## CA证书
抓包应用内置的CA证书必须安装到系统中。android系统将CA证书分为用户CA证书和系统CA证书。

安卓7.0开始谷歌改变了网络安全策略。用户CA证书默认不被TSL/SSL连接信任，解决方法：1、安装系统CA证书(root)。2、虚拟机安装app抓包(virtualxpose)。

其他原因抓不到包：公钥证书固定、双向认证、非http协议抓包。

HttpCanary使用man-in-the-middle(MITM)技术抓取和解析TLS/SSL协议数据包，比如常见的HTTPS、WSS等加密请求，所以使用之前必须先安装HttpCanary根证书。
