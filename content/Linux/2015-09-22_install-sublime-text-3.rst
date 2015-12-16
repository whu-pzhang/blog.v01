Linux安装Sublime Text 3
#################################

:author: pzhang
:date: 2015-09-22
:category: Linux
:tags: Linux, 安装
:slug: install-sublime-in-Linux

.. contents::

Sublime Text 3作为一款跨平台的编辑器，支持多种插件，语法高亮齐全，而且还有vi模式，
在Window环境下一直用的是这款编辑器。本文记录在CentOS7下安装，配置Sublime Text 3的过程。

安装
===========

从 \ `Sublime Text 3 <http://www.sublimetext.com/3>`_\官网选择相应的版本下载。

我下载的是64位的 \ ``tarball for other Linux distributions``\ 版本。

- 下载完成解压到\ ``opt``\目录下

::

    $ sudo tar jxvf sublime_text_3_build_3083_x64.tar.bz2 -C /opt/

- 然后建立软连接到 \ ``/usr/bin/``\目录下以便可以在终端启动

::

    $ sudo ln -s /opt/sublime_text_3/sublime_text /usr/bin/sublime

- 加入应用程序列表：

::

    $ sudo cp /opt/sublime_text_3/sublime_text.desktop /usr/share/applications/

修改该文本:

::

    $ sudo vi /usr/share/applications/sublime_text.desktop

将\ ``Exec``\和\ ``Icon``\这项改为：

::

    Exec=/opt/sublime_text_3/sublime_text
    Icon=/opt/sublime_text_3/Icon/48x48/sublime-text.png

其他的可以根据自己需求来修改。

修改完成后即可在 \ ``Gnome应用程序 > 编程``\ 中找到 sublime

- 加入右键菜单

::

    $ sublime ~/.local/share/applications/mimeapps.list

加入以下内容：

::

    text/plain=sublime.desktop;


基础设置
============

sublime text 3自带的配置已经可以用了，加上自己的一些设置会更好。

其设置方式为：

\ ``Preferences-->Settings-User``\ 打开配置文件，配置文件是以Python字典的格式存在的。

以下为个人的一些设置，每个选项的值可以在\ ``Preferences-->Settings-Default``\中查看

.. code-block:: Python

    {
        "color_scheme": "Packages/Monokai Extended/Monokai Extended.tmTheme",   # 安装Monokai Extended后的主题设置
        "default_line_ending": "unix",                      # 以UNIX格式的"\n"为行结尾，而不是doc的 "\r\n"
        "font_face": "Yahei Consolas Hybrid",               # 字体设置
        "font_size": 13,                                    # 字号
        "highlight_line": true,
        "ignored_packages":
        [
        ],
        "line_padding_bottom": 1,
        "line_padding_top": 1,
        "show_encoding": true,                              # 状态栏显示编码
        "show_line_endings": true,
        "tab_size": 4,                                      # tabwidth: 4个空格
        "translate_tabs_to_spaces": true,
        "vintage_ctrl_keys": true
    }


插件管理
==========

管理插件最简单的方法就是安装包管理器。

安装
-------

- **自动安装**

  - 组合键 \ ``ctrl+```\或者 \ ``View > Show Console``\打开控制台，输入以下 Python 代码

.. code-block:: python

    import urllib.request,os,hashlib; h = '2915d1851351e5ee549c20394736b442' + '8bc59f460fa1548d1514676163dafc88'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)

这段代码会下载 \ ``Package Control.sublime-package``\到 Installed Packages 文件夹（若没有的话会创建）。


- **手动安装**

  - Preferences > Browse Packages
  - 进入 Installed Packages 文件夹
  - 下载 \ `Package Control.sublime-package <https://packagecontrol.io/Package%20Control.sublime-package>`_\ 至该文件夹
  - 重启 Sublime Text 3
  
使用
----------

组合键 \ ``Ctrl+Shift+P``\打开包管理器，输入\ ``PCI``\即可搜索安装插件包

常用插件
---------

- IMESupport: 解决中文输入框不跟随的问题
- Monokai Extended: 增强的Monokai主题
- MinimalFortran: Fortran语法高亮


参考
=====
#. `Sublime Text 3 Documentation <http://www.sublimetext.com/docs/3/>`_
#. `Sublime Text Unofficial Documentation <http://docs.sublimetext.info/en/latest/index.html>`_
#. `Package Control <https://packagecontrol.io/installation>`_

