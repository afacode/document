## 问题原因
用了非标准的时间字符串去 new Date(), 类似 '2019-04-24'
在 Chrome 中是没有问题的, 但是 Safari 中不行, 其它浏览器未测试

## 如何避免问题
应该使用标准的时间格式, 包括:
* MM-dd-yyyy
* yyyy/MM/dd
* MM/dd/yyyy
* MMMM dd, yyyy
* MMM dd, yyyy

## 非标准时间转换
```
let time = '2019-04-24';
time = time.replace(/-/g, '/');
let date = new Date(time);
```