.. _topics-index:

==============================
Scrapy |version| 中文文档
==============================

Scrapy是一个快速、高效率的网络爬虫框架，用于抓取web站点并从页面中提取结构化的数据。
Scrapy被广泛用于数据挖掘、监测和自动化测试。

获得帮助
============

遇到困难了？我们乐意帮你解决！

* 试试 :doc:`FAQ <faq>` -- 这里有一些常见问题的解答。
* 寻找具体信息? 试试 :ref:`genindex` or :ref:`modindex`。
* 在 `StackOverflow using the scrapy tag`_ 里提问或搜索问题。
* 在 `Scrapy subreddit`_ 里提问或搜索问题。
* 在 `scrapy-users mailing list`_ 文档里搜索问题。
* 在 `#scrapy IRC channel`_ 上提问。
* 提交Scrapy错误报告请点击 `issue tracker`_。

.. _scrapy-users mailing list: https://groups.google.com/forum/#!forum/scrapy-users
.. _Scrapy subreddit: https://www.reddit.com/r/scrapy/
.. _StackOverflow using the scrapy tag: https://stackoverflow.com/tags/scrapy
.. _#scrapy IRC channel: irc://irc.freenode.net/scrapy
.. _issue tracker: https://github.com/scrapy/scrapy/issues


第一步
===========

.. toctree::
   :caption: 第一步
   :hidden:

   intro/overview
   intro/install
   intro/tutorial
   intro/examples

:doc:`intro/overview`
    了解什么是Scrapy,它是如何帮助你的。

:doc:`intro/install`
    在你的电脑上安装Scrapy。

:doc:`intro/tutorial`
    编写你的第一个Scrapy项目。

:doc:`intro/examples`
    通过运行一个内置的Scrapy例程进一步学习。

.. _section-basics:

基本概念
==============

.. toctree::
   :caption: 基本概念
   :hidden:

   topics/commands
   topics/spiders
   topics/selectors
   topics/items
   topics/loaders
   topics/shell
   topics/item-pipeline
   topics/feed-exports
   topics/request-response
   topics/link-extractors
   topics/settings
   topics/exceptions


:doc:`topics/commands`
    了解如何通过命令行管理Scrapy项目。

:doc:`topics/spiders`
    定义网站爬虫规则。

:doc:`topics/selectors`
    使用Xpath从网页中提取数据。

:doc:`topics/shell`
    在交互式环境中测试解析程序。

:doc:`topics/items`
    定义你想要获取的数据。

:doc:`topics/loaders`
    将提取的数据填充到项目中。

:doc:`topics/item-pipeline`
    处理和保存抓取到的数据。

:doc:`topics/feed-exports`
    将你抓取到的数据以不同的方式输出储存。

:doc:`topics/request-response`
    使用不同的类来实现HTTP的请求和响应。

:doc:`topics/link-extractors`
    便捷的类，用于提取页面中的超链接并继续跟进。

:doc:`topics/settings`
    了解如何配置Scrapy和查看所有的 :ref:`可用配置 <topics-settings-ref>`.

:doc:`topics/exceptions`
    查看所有可用的异常及其含义。


内置服务
=================

.. toctree::
   :caption: 内置服务
   :hidden:

   topics/logging
   topics/stats
   topics/email
   topics/telnetconsole
   topics/webservice

:doc:`topics/logging`
    了解如何在Scrapy上使用Python的内置日志。

:doc:`topics/stats`
    收集有关你的抓取爬虫的统计数据。

:doc:`topics/email`
    当某些事件发生时发送电子邮件通知。

:doc:`topics/telnetconsole`
    使用内置的Python控制台检查正在运行的爬虫器。

:doc:`topics/webservice`
    使用web服务监视和控制爬虫程序。


解决具体问题
=========================

.. toctree::
   :caption: 解决具体问题
   :hidden:

   faq
   topics/debug
   topics/contracts
   topics/practices
   topics/broad-crawls
   topics/developer-tools
   topics/leaks
   topics/media-pipeline
   topics/deploy
   topics/autothrottle
   topics/benchmarking
   topics/jobs

:doc:`faq`
    获取最常见问题的答案。

:doc:`topics/debug`
    了解如何Debug调试你的Scrapy爬虫常见问题。

:doc:`topics/contracts`
    了解如何使用约束条件来测试你的爬虫爬虫器。

:doc:`topics/practices`
    熟悉一些Scrapy常见的实践案例。

:doc:`topics/broad-crawls`
    优化Scrapy去并行爬取大量的域名。

:doc:`topics/developer-tools`
    学习如何使用浏览器的开发工具抓取。

:doc:`topics/leaks`
    学习查找和删除爬虫器中的内存泄漏。

:doc:`topics/media-pipeline`
    从抓取到数据中下载你在item中定义过的文件和图片。

:doc:`topics/deploy`
    部署你的Scrapy爬虫器并在远程服务器上运行它们。

:doc:`topics/autothrottle`
    根据负载动态调整爬虫速度。

:doc:`topics/benchmarking`
    检查一下Scrapy在硬件上的性能。

:doc:`topics/jobs`
    学习如何暂停并继续大型的爬虫器。

.. _extending-scrapy:

Scrapy 扩展
================

.. toctree::
   :caption: Scrapy 扩展
   :hidden:

   topics/architecture
   topics/downloader-middleware
   topics/spider-middleware
   topics/extensions
   topics/api
   topics/signals
   topics/exporters


:doc:`topics/architecture`
    理解Scrapy的架构。

:doc:`topics/downloader-middleware`
    定制爬虫页面如何请求和下载。

:doc:`topics/spider-middleware`
    自定义你的爬虫器的输入和输出。

:doc:`topics/extensions`
    使用你自定义的函数扩展Scrapy。

:doc:`topics/api`
    在扩展和中间件上使用它来扩展Scrapy功能。

:doc:`topics/signals`
    查看所有可用的信号以及如何使用它们。

:doc:`topics/exporters`
    快速导出你的抓取项目到一个文件(XML, CSV等)。


其他
============

.. toctree::
   :caption: 其他
   :hidden:

   news
   contributing
   versioning

:doc:`news`
    看看在最近的Scrapy版本中发生了什么变化。

:doc:`contributing`
    了解如何为Scrapy仓库贡献代码。

:doc:`versioning`
    了解Scrapy的版本控制和API的稳定性。
