有限差分原理及FDELMODC软件使用笔记
#####################################

:author: pzhang
:date: 2015-12-22
:category: 地震学软件
:tags: math
:slug: finite-difference
:mathjax: true

.. contents::

忠实地模拟观测地震数据需要包含地球信息的“真实”计算模型，例如：各向异性介质、
层状或块状介质间的非平面界面、存在速度/密度/质量因子梯度的层状介质以及包含自由表面
地形起伏的模型等等。其中，对于各向异性介质中地震波的模拟，在结构研究以及勘探地震应用
中扮演着越来越重要的角色。

在所有模拟地震波传播的方法中，有限差分(Finite-Difference)方法仍然占有主导地位，并且在
地震勘探工业界以及结构模拟中扮演者越来越重要的角色。这是因为FD方法能够在较为复杂的模型中
提供波场传播相对精确的“完全”解，与此同时，其计算效率也相对较高，并且比较容易并行化。

在FD中，计算域被空间-时间网格所覆盖。介质性质由波场在网格点上的值以及位置所描述。
网格点上的空间和时间偏导由所谓的FD方程所近似。

FD方程及其数值解有三个基本性质————一致性、稳定性以及收敛性。这些性质须在数值计算前分析。

原则上，FD方法既能在时间域也能在频率域实现。

有限差分公式
==================

我们从地震学上的动力学方程开始

.. math::
    \rho \frac{\partial^2 u}{\partial t^2} = \nabla \cdot \tau

待补充部分。。。。。

用速度对时间的偏导\ :math:`v`\和对空间的
偏导（应力）\ :math:`\tau = \mu \partial v/ \partial x`\来分别代替位移\ :math:`\mu`\可以
避免求二次导数。这样的话方程变成以下形式

.. math::
    \frac{\partial v}{\partial t} = \frac{1}{\rho (x)} \frac{\partial \tau}{\partial x}


Fdelmodc软件包安装及使用
============================

安装
---------------

去到 http://janth.home.xs4all.nl/Software/Software.html 即可下载

这里不光有2D声波/粘弹性波有限差分程序还包括一些叠前深度偏移程序。我们直接下载那个大包，
即 **2D acoustic/visco-elastic finite difference wavefield modeling + tools for 2D gridded model building and wavelet definition + 2D wavefield extrapolation/migration.**

下载后解压::

    $ tar zxvf OpenSourceAll.tgz

<<<<<<< HEAD
得到文件夹\ ``OpenSource``\ ，我选择的是将其放到\ ``$HOME``\ 目录下以便翻阅。::
=======
得到文件夹\ ``OpenSource``\ ，我选择的是将其放到\ ``$HOME``\ 目录下以便翻阅
::
>>>>>>> 215929f12f310af960d8dd459fc40f250831504b

    $ mv OpenSource ~/
    $ cd ~/OpenSource

这时需要修改\ ``Make_include``\ 和\ ``Makfile``\ 两个文件::

    # 将ROOT目录改为自己所放的源码目录，我的如下
    ROOT=/home/pzhang/OpenSource

改完保存，直接make即可::

    $ make
    # 添加环境变量
    $ echo 'export $PATH=/home/pzhang/OpenSource:${PATH}' >> ~/.bashrc
    $ exec -l $SHELL

至此，fdelmodc软件包就安装完成。
值得注意的是，此包中Makefile只编译FFTlib、fdelmodc以及utils中的程序，
若要安装另外的偏移程序，则需要另外自行编译。

使用
---------------

软件目录下有\ ``demos``\ 文件夹,里面有一些例子可供参考。






