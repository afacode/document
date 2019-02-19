## scrapy
网友 [源码地址](https://blog.csdn.net/qq_41682050/article/details/81043508)
[https://github.com/cuanboy/ScrapyProject](https://github.com/cuanboy/ScrapyProject)
* 系列

地址 [http://books.toscrape.com/](http://books.toscrape.com/) <br>
> 目标 抓取电商网站上的图书名称和价格

1. 新建项目 `scrapy startproject example` 
2. 新建爬虫 `scrapy genspider book_spider` 或者 在 example\example\spiders 新建 book_spider.py
3. 编写 `book_spider.py` 
4. 运行并保存 在根路径 `scrapy crawl book_spider -o books.csv`

```python
# -*- coding: utf-8 -*-
import scrapy


class BookSpider(scrapy.Spider):
  name = "books"

  start_urls = ['http://books.toscrape.com/']
  
  def parse(self, response):
    # 解析网页
    for book in response.css('article.product_pod'):
      name = book.xpath('./h3/a/@title').extract_first()
      price = book.css('p.price_color::text').extract_first()
      yield {'name':name, 'pirce':price}

    # 实现翻页
    next_url = response.css('ul.pager li.next a::attr(href)').extract_first()
    if next_url:
      next_url = response.urljoin(next_url)
      yield scrapy.Request(next_url, callback=self.parse)
```

> Request objects
```py
Request(url[, callback, method='GET', headers, body, cookies, meta, encoding='utf-8', priority=0, dont_filter=False, errback])
```
> Response
* TextResponse
* HtmlResponse
* XmlResponse
	* xpath(query)
	* css(query)
	* urljoin(url)

> spider开发流程
* 打开页面
```js
start_urls = ['http://books.toscrape.com/']
或者
def start_requests(self):
	urls = [ #爬取的链接由此方法通过下面链接爬取页面
		 'http://lab.scrapyd.cn/page/1/',
		 'http://lab.scrapyd.cn/page/2/',
	]
	for url in urls:
		#发送请求
		 yield scrapy.Request(url=url, callback=self.parse)
def parse
```
## 数据提取 css选择器、xpath选择器、正则
css选择器、xpath选择器、正则

> css 
* 提取属性我们是用：标签名::attr(属性名) url表达式就是：a::attr(href)
* class选择器 .page-navigator id=page-navigator #page-navigator
* response.css(".page-navigator a::attr(href)").extract()
* extract_first()  extract()
* 标签内容的提取 ::text 
* 所有文字 *::text

> xpath
xpath 提供许多函数,如数字、字符串、时间、日期、统计等
* string(arg)：返回参数的字符串值
* contains(str1, str2)：判断str1中是否包含str2，boolean

## 全局命令
* scrapy startproject（创建项目）、 scrapy strartproject scrapychina
* scrapy crawl XX（运行XX蜘蛛）、
* scrapy shell http://www.scrapyd.cn

* startproject
* genspider
* settings
* runspider
* shell
* fetch
* view
* version

## 项目命令
* crawl
* check
* list
* edit
* parse
* bench

## 使用Item封装数据
```
items.py

import scrapy

class BookItem(scrapy.Item):
	name = scrapy.Field()

```
## 下载文件和图片 FilesPipeline ImagesPipeline
scrapy.pipelines.images.ImagesPipeline

## 登录 FormRequest










