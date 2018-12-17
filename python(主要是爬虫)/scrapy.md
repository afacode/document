## scrapy
网友 [源码地址](https://blog.csdn.net/qq_41682050/article/details/81043508)
* 系列

地址 [http://books.toscrape.com/](http://books.toscrape.com/) <br>
> 目标 抓取电商网站上的图书名称和价格

1. 新建项目 `scrapy startproject example` 
2. 新建爬虫 `scrapy genspider book_spider` 或者 在 example\example\spiders 新建 book_spider.py
3. 编写 `book_spider.py` 
4. 运行并保存 在根路径 `scrapy crawl book_spider -o books.csv`

```python
# __*__ codind: utf-8 __*__
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