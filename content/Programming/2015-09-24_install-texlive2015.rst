TexLive 2015安装
#################

:author: pzhang
:date: 2015-09-24
:category: Linux
:tags: Linux, 安装, LaTex
:slug: install-texlive2015

.. contents::

Tex Live是TEX及其相关程序在 GNU/Linux及其他类Unix系统上的发行版本。

是由国际 TeX 用户组织 TUG 开发的 Tex系统，支持不同的操作系统平台。其Window版本又称 fpTex, Linux下即著名的 teTeX。

本文记录 Tex Live 2015下Linux下的安装

依赖
======

- 安装过程中需要调用Perl模块 \ ``Digest::MD5``\来检测ISO文件的完整性；
- 升级过程中界面需要调用Perl的模块 \ ``Tk``\；

Red Hat:

::

    $ sudo yum install perl-Digest-MD5 perl-Tk

Debian:

::
    
    $ sudo apt-get install libdigest-perl-md5-perl perl-tk

下载
=====

下载方式有多种

官方镜像： http://mirrors.ctan.org/systems/texlive/Images/texlive2015.iso

教育网可以选择学校的镜像站，如科大或者华科的：

http://mirror.hust.edu.cn/CTAN/systems/texlive/Images/texlive2014.iso

或者

http://mirrors.ustc.edu.cn/CTAN/systems/texlive/Images/texlive2014.iso

Linux下可以用 Wget， Window下可以用迅雷，怎么快怎么来。

安装
======

Tex Live 的安装需要 Perl 的支持。

先挂载镜像

.. code-block:: bash

    $ su
    # mount -o loop texlive2015.iso /mnt/
    # cd /mnt
    # ./install-tl

这时会出现安装选项，输入\ ``I``\ 直接默认安装（ \ ``/usr/local``\ ）

需要更改安装路径可以输入 \ ``D``\ , 这时输入你要安装的目录路径即可。

本人是安装在 /opt 目录下 

添加环境变量
============

添加以下语句到 \ ``~/.bashrc``\中

.. code-block:: bash

    # TeX Live 2014
    export MANPATH=${MANPATH}:/opt/texlive/2015/texmf-dist/doc/man
    export INFOPATH=${INFOPATH}:/opt/texlive/2015/texmf-dist/doc/info
    export PATH=${PATH}:/opt/texlive/2015/bin/x86_64-linux

然后执行 \ ``source ~/.bashrc``\ 或者 \ ``exec $SHELL``\ 使其生效。

卸载镜像
========

安装完成后，要记得卸载镜像

.. code-block:: bash
    
    sudo umount /mnt/

升级更新
=============

.. code-block:: bash

    $ sudo tlmgr update --self      # 更新TeXLive包管理器
    $ sudo tlmgr update --all       # 更新TeXLive的全部包

默认下会自动搜索合适的镜像来更新，但也可以使用\ ``--repository``\选项指定了要使用哪一个CTAN镜像。

比如USTC镜像

.. code-block:: bash

    $ sudo tlmgr update --self --repository http://mirrors.ustc.edu.cn/CTAN/systems/texlive/tlnet/

或者阿里云镜像

.. code-block:: bash

    $ sudo tlmgr update --self --repository http://mirrors.aliyun.com/CTAN/systems/texlive/tlnet/

此外还可以通过图形界面来升级：

.. code-block:: bash

    $ sudo tlmgr --gui --gui-lang zh_CN


参考
==========

#. `Linux下安装TexLive2015 <http://seisman.info/install-texlive-under-linux.html>`_