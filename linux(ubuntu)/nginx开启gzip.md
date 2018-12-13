### nginx 开启 gzip
Gzip是网页的一种网页压缩技术，经过gzip压缩后，页面大小可以变为原来的30%甚至更小。更小的网页会让用户浏览的体验更好，速度更快。gzip网页压缩的实现需要浏览器和服务器的支持

* gzip : 该指令用于开启或 关闭gzip模块。
* gzip_buffers : 设置系统获取几个单位的缓存用于存储gzip的压缩结果数据流。
* gzip_comp_level : gzip压缩比，压缩级别是1-9，1的压缩级别最低，9的压缩级别最高。压缩级别越高压缩率越大，压缩时间越长。
* gzip_disable : 可以通过该指令对一些特定的User-Agent不使用压缩功能。
* gzip_min_length:设置允许压缩的页面最小字节数，页面字节数从相应消息头的Content-length中进行获取。
* gzip_http_version：识别HTTP协议版本，其值可以是1.1.或1.0.
* gzip_proxied : 用于设置启用或禁用从代理服务器上收到相应内容gzip压缩。
* gzip_vary : 用于在响应消息头中添加Vary：Accept-Encoding,使代理服务器根据请求头中的Accept-Encoding识别是否启用gzip压缩。


```bash
http {
	..............
	gzip on;
	gzip_types text/plain application/javascript text/css;
	..............
}
```
gzip_types 客户端访问网页时，对文本、JavaScript 和CSS文件进行压缩输出

