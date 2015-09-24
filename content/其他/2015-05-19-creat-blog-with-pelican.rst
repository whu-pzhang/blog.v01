用Pelican搭建静态博客
#########################

:author: pzhang
:date: 2015-05-19
:category: 其他
:tags: Pelican, Python, reStructuredText
:slug: construct-blog-with-pelican

.. contents::

以前比较小白，什么都不太懂，地球物理方面的博客看过 `SeisMan <seisman.info>`_ ，也就萌生了搭建自己博客的想法。

Google 之后，发现搭建静态博客很火的是 **Jekyll** ，但这个是 Ruby 编写的，没学过，也没啥兴趣。所以选择了 Python 实现的 **Pelican**
来搭建自己的博客。

介绍
=====

    **"Pelican" is an anagram of calepin, which means "notebook" in French.**

Pelican 是使用 Python 编写的静态博客生成器。

* 支持 MarkDown、reStructuredText 和 AsiiDoc 等格式
* 使用简单的命令行工具生成站点内容
* 易于结合版本控制系统及其web应用搭配使用
* 全静态输出，易于部署
* 支持 Disqus 评论系统, RSS 和 Atom 输出, 插件, 主题, 代码高亮等功能
* 采用 Jajin2 模板引擎, 可以很容易的更改模板。


安装
=====

Pelican3.5.0 支持 Python2.7.x 和 Python3.3+

最简单的安装方法就是利用 pip。我的 Python 环境为 Anaconda3-2.2.0，基于 Python3.4.3 的科学计算包。

在安装好 Python 环境后

.. code-block:: sh

    pip install pelican
    pip install markdown    # 让其支持 markdown 语法来写博客

这样就安装好了 Pelican

开始博客
===========

生成站点
---------

通过 `pelican-quickstart` 命令快速建立一个骨架。

.. code-block:: sh

    mkdir /path/to/your/blog
    cd /path/to/your/blog
    pelican-quickstart

回答一系列问题后，就创建完成了。结构如下：

.. code-block:: sh

    blog/
    ├── content  # 博客内容目录
    ├── output   # 输出文件目录
    ├── develop_server.sh   # 方便开启测试服务器
    ├── Makefile            # 方便管理博客的Makefile
    ├── pelicanconf.py      # 主配置文件
    └── publishconf.py      # 主发布文件，可删除


写第一篇博文
------------

在 content 目录下 新建一个 test.rst 文件：

.. code-block:: sh

    :title: My First Review
    :daate: 2010-12-03 10:20
    :category: Review

    Following is a review of my favorite mechanical keyboard

然后，在 blog 目录下运行

.. code-block:: sh

    pelican content

即可生成输出文件。

然后

.. code-block:: sh

    make serve

就可以开启本地服务。在 Http://localhost:8000 即可通过浏览器浏览啦！


更换主题
=========

默认主题丑哭::>_<::~ 不能忍，必须换主题！

Pelican 本身也提供了一些主题供下载： https://github.com/getpelican/pelican-themes

里面主题基本都提供了预览，也可以到 http://www.pelicanthemes.com/ 查看各个主题的预览。
根据自己喜好选择。找到自己喜欢的主题后，可以用 git 克隆到博客目录下，比如：

.. code-block:: sh

    git clone https://github.com/talha131/pelican-elegant.git

然后再 pelicanconf.py 配置文件中添加或修改 THEME 为 pelican-elegant

.. code-block:: python

    THEME = 'pelican-elegant'

重新执行

.. code-block:: sh

    pelican content
    make serve

即可。


添加插件
=========

要使用插件，首先你得去下载（废话！） Pelican插件 https://github.com/getpelican/pelican-plugins

在根目录下建立 `plugins` 的文件夹，将下载的插件放进去。

然后打开 `pelicanconf.py`，定义插件目录和启动插件。

.. code-block:: python

    # Plugins
    PLUGIN_PATHS = ['plugins', ]
    PLUGINS = [
        'render_math',          # 支持数学公式渲染
        'extract_toc',          # 将toc从content提取出来，单独处理
        'neighbors',            # 邻居导航,也就是我们常说的上一篇下一篇文章
        'related_posts',        # 相关文章,根据tags判断的
        'sitemap',              # 地图,你懂的
    ]



添加评论功能
==============

评论功能通过 Disqus 实现，因为暂时只是本地博客，等上线时再弄。。。。。


部署博客上线
==============

需要购买独立域名和 DNS 解析。

在 `Godaddy <https://www.godaddy.com>`_ 上可以购买域名，为什么选择这个呢？因为支持支付宝付款啊！ 没有信用卡伤不起::>_<:: ~

后续。。。。。。。


参考
=======

#. `pelican搭建属于自已的博客 <http://www.tuicool.com/articles/IFbEBj>`_
#. `使用Pelican打造静态博客 <http://www.linuxzen.com/shi-yong-pelicanda-zao-jing-tai-bo-ke.html>`_
#. `Pelican Dos <http://docs.getpelican.com/en/3.5.0/index.html>`_
#. `Pelican中文文档 <http://pelican-docs-zh-cn.readthedocs.org/en/latest/getting_started.html>`_
#. `Elegant <http://oncrashreboot.com/elegant-best-pelican-theme-features>`_


