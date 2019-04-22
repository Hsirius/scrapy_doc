.. _intro-overview:

==================
预览Scrapy
==================

Scrapy是一个用于抓取和提取web站点的结构化数据的应用框架
，可用于广泛的有用的应用程序，如
数据挖掘、信息处理或历史档案。

尽管Scrapy最初是为 `web scraping`_ 而设计的, 但它也可以用API提取数据
 (例如 `Amazon Associates Web Services`_) 或者作为一个通用的网络爬虫。

浏览爬虫器例子
=================================

为了向你展示Scrapy带来了什么，我们将用一个Scrapy最简单的方法带你浏览一个
Scrapy爬虫器的例子。

下面是这个爬虫器的代码，它可以从 http://quotes.toscrape.com 网站上抓取名人名言,
根据以下的页码::

    import scrapy


    class QuotesSpider(scrapy.Spider):
        name = 'quotes'
        start_urls = [
            'http://quotes.toscrape.com/tag/humor/',
        ]

        def parse(self, response):
            for quote in response.css('div.quote'):
                yield {
                    'text': quote.css('span.text::text').get(),
                    'author': quote.xpath('span/small/text()').get(),
                }

            next_page = response.css('li.next a::attr("href")').get()
            if next_page is not None:
                yield response.follow(next_page, self.parse)


把代码放到一个Python文件中，文件名如 ``quotes_spider.py``
然后用 :command:`runspider` 命令运行这个爬虫器::

    scrapy runspider quotes_spider.py -o quotes.json


当它执行完毕后，你会发现目录下多了一个名为 ``quotes.json`` 的文件，文件内容
是以JSON格式储存的，其中包含名言和作者，格式如下（为了美观进行了排版）::

    [{
        "author": "Jane Austen",
        "text": "\u201cThe person, be it gentleman or lady, who has not pleasure in a good novel, must be intolerably stupid.\u201d"
    },
    {
        "author": "Groucho Marx",
        "text": "\u201cOutside of a dog, a book is man's best friend. Inside of a dog it's too dark to read.\u201d"
    },
    {
        "author": "Steve Martin",
        "text": "\u201cA day without sunshine is like, you know, night.\u201d"
    },
    ...]


在此期间发生了什么？
-------------------

当你运行 ``scrapy runspider quotes_spider.py`` 命令之后, Scrapy 会找到定义的爬虫器，
然后通过爬虫引擎运行它们。

爬虫是通过发送请求给 ``start_urls`` 属性中定义的 url(在本例中，就是 *humor* 分类下的
那些 url)
然后调用默认的回调函数 ``parse``，并把响应的对象作为参数传给它。在 ``parse`` 回调函数中，
我们使用 CSS 选择器遍历quote元素，并把解析的引用名言和作者生成一个字典通过生成器返回，
寻找并请求下一个链接且继续使用 ``parse`` 方法作为回调函数。

这里你可以注意到Scrapy的一个主要的优势：:ref:`请求的调度是异步的 <topics-architecture>`。
这意味着Scrapy不需要等待一个请求被完成处理，它可以同时发送另一个请求或者做一些其他事情。
这也同样表明当一些请求失败或者处理发生错误时其他请求可以继续进行下去。

虽然这使得你可以非常快速地进行爬虫（同时可以并发多个请求，在可承载的压力下），
不过Scrapy同样提供了优雅的爬虫方法，:ref:`简单地配置即可 <topics-settings-ref>`。
你也可以设置一些其他的参数，比如给下载器设置一个延迟，限制每个域名或者代理IP的并发量，
甚至可以让Scrapy自动根据请求的响应情况进行 :ref:`限流 <topics-autothrottle>`。

.. note::

    本例中 :ref:`导出 <topics-feed-exports>` 了一个JSON文件,你可以轻易地更改输出格式（比如XML 或者 CSV）或者
    储存到后端（比如FTP或者`Amazon S3`_ ）。你同样可以定义一个 :ref:`item 管道 <topics-item-pipeline>`
    把这些 item 储存到数据库中。

.. _topics-whatelse:

What else?
==========

你已经知道了如何用Scrapy从一个站点提取和储存item，但是仅仅是很浅显的了解它。
Scrapy还为爬虫提供了很多强大的功能，比如：

* 用内置的 CSS 选择器和 XPath 语法从 HTML/XML 源中 :ref:`选择和解析数据 <topics-selectors>`。
  甚至可以在其中使用正则表达式来辅助解析。

* 一个 :ref:`交互控制台 <topics-shell>` (IPython aware)，可以用CSS选择器和XPath语法来解析数据，
  在编写和调试爬虫器的时候十分方便。

* 内置支持 :ref:`生成输出 <topics-feed-exports>` 多种格式文件 (JSON, CSV, XML) 和多种后端存储 (FTP,
  S3, 本地文件系统)。

* 强大的编码支持和检测,用于处理外部的，不标准的或者损坏的编码。

* :ref:`强大的扩展 <extending-scrapy>`, 允许你植入自己写的函数，:ref:`信号 <topics-signals>` 和定义好
  的 API(中间件, :ref:`扩展 <topics-extensions>`, 以及 :ref:`管道 <topics-item-pipeline>`)。

* 广泛的内置扩展功能和中间件处理:

  - 处理cookies和session
  - HTTP特性包括压缩、身份验证以及缓存
  - 模拟user-agent
  - robots.txt协议
  - 爬虫深度限制
  - 更多

* 一个 :ref:`远程控制台 <topics-telnetconsole>` 用于连接运行在Scrapy进程的Python控制台,
  以便于自省和调试爬虫程序。

* 还有其他好东西，比如从`Sitemaps`_ and XML/CSV 源导入可复用的爬虫, 一个可以自动下载图片（或者其他和items关联的媒体文件）的 
  :ref:`媒体中间件<topics-media-pipeline>`,一个DNS缓存解析器, 以及更多!

下一步?
============

你下一步要做的就是 :ref:`下载 Scrapy <intro-install>`,
:ref:`跟着教程 <intro-tutorial>` 去创建一个完整的Scrapy工程 并且 `join the community`_. 感谢您的关注!

.. _join the community: https://scrapy.org/community/
.. _web scraping: https://en.wikipedia.org/wiki/Web_scraping
.. _Amazon Associates Web Services: https://affiliate-program.amazon.com/gp/advertising/api/detail/main.html
.. _Amazon S3: https://aws.amazon.com/s3/
.. _Sitemaps: https://www.sitemaps.org/index.html
