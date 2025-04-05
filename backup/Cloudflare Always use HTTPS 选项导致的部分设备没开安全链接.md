<!-- ##{"timestamp":1743850589}## -->

# BB
之前 Cloudflare 添加域名都会有自动设置的初始化指引，其中就包含 `Always use HTTPS` 
最近没了我以为默认就选上了也没特意去找过选项在哪里没注意过

然后刚有个网站发现电脑端访问正常，移动端 Safari 也正常，移动端的 Chrome 却报错感叹号提示不安全，也就是停在了纯 HTTP 状态
一开始以为是 SSL 证书的模式 `SSL/TLS encryption mode` 问题，改了发现也没用

然后突然想起曾经有过强制 HTTPS 的东西，找了下位置打开后就解决了

SSL/TLC - Edge Certificates - Always use HTTPS

-----------------------------------------------------------------------------
# 顺便记录下绑定了域名后初始化的几个建议选项
## 1. 自动证书模式
SSL/TLC - Overview - SSL/TLS encryption - Configure - Automatic SSL/TLS (default) - Select - Save

## 2. 强制使用 HTTPS + HSTS
SSL/TLC - Edge Certificates - Always use HTTPS
HTTP Strict Transport Security (HSTS) - Enable HSTS（非必要）