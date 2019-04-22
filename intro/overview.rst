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

为了向您展示Scrapy带来了什么，我们将用一个Scrapy最简单的方法带您浏览一个
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


当它执行完毕后，您会发现目录下多了一个名为 ``quotes.json`` 的文件，文件内容
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

When you ran the command ``scrapy runspider quotes_spider.py``, Scrapy looked for a
Spider definition inside it and ran it through its crawler engine.

The crawl started by making requests to the URLs defined in the ``start_urls``
attribute (in this case, only the URL for quotes in *humor* category)
and called the default callback method ``parse``, passing the response object as
an argument. In the ``parse`` callback, we loop through the quote elements
using a CSS Selector, yield a Python dict with the extracted quote text and author,
look for a link to the next page and schedule another request using the same
``parse`` method as callback.

Here you notice one of the main advantages about Scrapy: requests are
:ref:`scheduled and processed asynchronously <topics-architecture>`.  This
means that Scrapy doesn't need to wait for a request to be finished and
processed, it can send another request or do other things in the meantime. This
also means that other requests can keep going even if some request fails or an
error happens while handling it.

While this enables you to do very fast crawls (sending multiple concurrent
requests at the same time, in a fault-tolerant way) Scrapy also gives you
control over the politeness of the crawl through :ref:`a few settings
<topics-settings-ref>`. You can do things like setting a download delay between
each request, limiting amount of concurrent requests per domain or per IP, and
even :ref:`using an auto-throttling extension <topics-autothrottle>` that tries
to figure out these automatically.

.. note::

    This is using :ref:`feed exports <topics-feed-exports>` to generate the
    JSON file, you can easily change the export format (XML or CSV, for example) or the
    storage backend (FTP or `Amazon S3`_, for example).  You can also write an
    :ref:`item pipeline <topics-item-pipeline>` to store the items in a database.


.. _topics-whatelse:

What else?
==========

You've seen how to extract and store items from a website using Scrapy, but
this is just the surface. Scrapy provides a lot of powerful features for making
scraping easy and efficient, such as:

* Built-in support for :ref:`selecting and extracting <topics-selectors>` data
  from HTML/XML sources using extended CSS selectors and XPath expressions,
  with helper methods to extract using regular expressions.

* An :ref:`interactive shell console <topics-shell>` (IPython aware) for trying
  out the CSS and XPath expressions to scrape data, very useful when writing or
  debugging your spiders.

* Built-in support for :ref:`generating feed exports <topics-feed-exports>` in
  multiple formats (JSON, CSV, XML) and storing them in multiple backends (FTP,
  S3, local filesystem)

* Robust encoding support and auto-detection, for dealing with foreign,
  non-standard and broken encoding declarations.

* :ref:`Strong extensibility support <extending-scrapy>`, allowing you to plug
  in your own functionality using :ref:`signals <topics-signals>` and a
  well-defined API (middlewares, :ref:`extensions <topics-extensions>`, and
  :ref:`pipelines <topics-item-pipeline>`).

* Wide range of built-in extensions and middlewares for handling:

  - cookies and session handling
  - HTTP features like compression, authentication, caching
  - user-agent spoofing
  - robots.txt
  - crawl depth restriction
  - and more

* A :ref:`Telnet console <topics-telnetconsole>` for hooking into a Python
  console running inside your Scrapy process, to introspect and debug your
  crawler

* Plus other goodies like reusable spiders to crawl sites from `Sitemaps`_ and
  XML/CSV feeds, a media pipeline for :ref:`automatically downloading images
  <topics-media-pipeline>` (or any other media) associated with the scraped
  items, a caching DNS resolver, and much more!

What's next?
============

The next steps for you are to :ref:`install Scrapy <intro-install>`,
:ref:`follow through the tutorial <intro-tutorial>` to learn how to create
a full-blown Scrapy project and `join the community`_. Thanks for your
interest!

.. _join the community: https://scrapy.org/community/
.. _web scraping: https://en.wikipedia.org/wiki/Web_scraping
.. _Amazon Associates Web Services: https://affiliate-program.amazon.com/gp/advertising/api/detail/main.html
.. _Amazon S3: https://aws.amazon.com/s3/
.. _Sitemaps: https://www.sitemaps.org/index.html
