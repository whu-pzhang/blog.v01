深刻理解傅里叶变换
###################

:date: 2015-07-13
:author: pzhang
:category: Basic_Seis
:tags: math
:slug: fourier-transform
:mathjax: true

.. contents::

一直对傅里叶变换理解不是很深，总是感觉很模糊。暑假正好复习了一遍。总结一下。

本文主要侧重于离散序列的傅里叶变换，对于连续信号不作讨论。

基本概念
============

正交
------------

正交向量
~~~~~~~~~~~

所谓 **正交** ，对一对向量来说，就是它们的内积为 0，即

.. math::
    \mathbf {V_x V_y = 0}

两个向量正交，也就是说其中任意一个向量都完全没有另一个的分量。
由两两正交的向量组成的向量集合称为 **正交基** 。若每个向量都为单位向量，则称其为向量空间的 **标准正交基**。

正交函数集
~~~~~~~~~~~

将向量内积推广到函数上，就可以得出正交函数的概念。
对于实变函数\ :math:`f(x)`\和\ :math:`g(x)`\，假设有这样两个向量：

- 对应\ :math:`f(x)`\的 \ :math:`\mathbf{u} = [f(0), f(1), f(2), ..., f(N-1)]`\
- 对应\ :math:`g(x)`\的 \ :math:`\mathbf{v} = [g(0), g(1), g(2), ..., g(N-1)]`\

这两个向量的内积为：

.. math::
    \mathbf{u \cdot v} = f(0)g(0) + f(1)g(1) + f(2)g(2) + ... + f(N-1)g(N-1)

这种求和过渡到连续情况下就是积分了。所以若在定义域\ :math:`(t_1, t_2)`\上的两个函数
\ :math:`f_1(t)`\和\ :math:`f_2(t)`\满足

.. math::
    \int^{t_2}_{t_1} f_1(x) f_2(x) {\rm d}x = 0

我么就称这两个函数正交。

若 N 个函数相互正交的函数组成一个函数集，在区间上满足

.. math::
    \int^{t_2}_{t_1} f_i(x) f_j(x) {\rm d}x = 
    \begin{cases}
    0 & i \neq j \\
    K_i & i = j
    \end{cases}

就称此函数集为区间\ :math:`(t_1, t_2)`\上的 **正交函数集**。

而若该区间上次函数集是唯一的正交集，也就是说不存在之外的函数\ :math:`f(x) ( \neq 0)`\满足

.. math::
    \int^{t_2}_{t_1} f(x) f_i(x) {\rm d}x = 0   \quad (i=1,2,3,4,...)

则称该正交函数集为 **完备正交函数集**。

例如：三角函数集合复指数函数集

卷积
-----------

任何离散时间序列都可以表示成移位的离散时间单位脉冲序列\ :math:`\delta [n-k]`\的线性组合：

.. math::
    x[n] = \sum_{k=-\infty}^{+\infty} {x[k] \delta [n-k]}

这个性质称为离散时间单位脉冲序列的筛选性质(sifting property)。因为序列\ :math:`\delta [n-k]`\
仅当\ :math:`k=n`\时为非零，该式就相当于把\ :math:`x[n]`\序列作了筛选，仅保留对应于\ :math:`k=n`\
时的值。

对于离散时间线性系统来说，其对\ :math:`x[n]`\的响应就是系统对前面所示的这些单位移位脉冲序列的每一个响应加权后的叠加。

我们令\ :math:`h_k[n]`\为系统对移位单位脉冲\ :math:`\delta [n-k]`\的响应，根据线性系统的叠加性质，\
该系统对输入\ :math:`x[n]`\的响应\ :math:`y[n]`\就是这些基本响应的加权线性组合:

.. math::
    y[n] = \sum_{k=-\infty}^{+\infty} {x[k] h_k[n]}

与此同时，若该线性系统是时不变的，那么这些移位单位脉冲的响应也都互相作了移位。具体说来，\
因为\ :math:`\delta [n-k]`\是\ :math:`\delta [n]`\的时间移位，那么其响应\ :math:`h_k[n]`\ \
也就是\ :math:`h_0[n]`\的一个时移，即：

.. math::
    h_k[n] = h_0[n-k]

为了简化，将\ :math:`h_k[n]`\的下标去掉，定义系统单位脉冲序列响应为：

.. math::
    h[n] = h_0[n]

也就是说，\ :math:`h[n]`\是LTI系统当输入为\ :math:`\delta [n]`\时的输出。这样的话，对LTI系统而言 \
输入\ :math:`x[n]`\的响应\ :math:`y[n]`\就为：

.. math::
    y[n] = \sum_{k=-\infty}^{+\infty} {x[k] h[n-k]}

这即为 **卷积**，用符号记作：

.. math::
    y[n] = x[k] \ast h[n]



周期函数傅里叶级数
=====================

对于一个周期为\ :math:`2l`\的函数，其 Fourier级数展开式为：

.. math::
    f(x) = \frac {a_0}{2} + \sum_{n=1}^{\infty} (a_n cos{\frac {n \pi x}{l}} + b_n sin{\frac {n \pi x}{l}})

式中

.. math::
    a_0 &= \frac{1}{l} \int_{-l}^{l} {f(x) {\rm d} x} \\
    a_n &= \frac{1}{l} \int_{-l}^{l} {f(x) cos{\frac {n \pi x}{l}} {\rm d} x} \\
    b_n &= \frac{1}{l} \int_{-l}^{l} {f(x) cos{\frac {n \pi x}{l}} {\rm d} x}

若\ :math:`f(x)`\为奇函数，这时\ :math:`f(x) cos{\frac {n \pi x}{l}}`\也为奇函数，\
\ :math:`a_n`\均为零，Fourier级数变成了正弦级数：


.. math::
    f(x) = \sum_{n=1}^{\infty} {b_n sin{\frac {n \pi x}{l}}}

同时，\ :math:`b_n`\可以写为：

.. math::
    b_n = \frac{2}{l} \int_{0}^{l} {f(x) cos{\frac {n \pi x}{l}} {\rm d} x}

若\ :math:`f(x)`\为偶函数，这时\ :math:`f(x) sin{\frac {n \pi x}{l}}`\为奇函数， \
\ :math:`b_n`\为零，Fourier级数变成了余弦级数：

.. math::
    f(x) = \frac {a_0}{2} + \sum_{n=1}^{\infty} {a_n cos{\frac {n \pi x}{l}}}

其中，\ :math:`a_0`\和\ :math:`a_n`\可简写为：

.. math::
    a_0 &= \frac {2}{l} \int_0^l {f(x) {\rm d}x} \\
    a_n &= \frac {2}{l} \int_0^l {f(x) cos{\frac {n \pi x}{l}} {\rm d}x}


离散傅里叶变换
=================

根据原信号的不同类型，可以把傅立叶变换分为四种类别：


+-----------------+------------------------------------------------------+
|非周期连续信号   | 傅里叶变换(Fourier Transform)                        |
+-----------------+------------------------------------------------------+
|周期性连续信号   |  傅里叶级数(Fourier Series)                          |
+-----------------+------------------------------------------------------+
|非周期离散信号   |  离散时域傅里叶变换(Discrete Time Fourier Transform) |
+-----------------+------------------------------------------------------+
|周期性离散信号   |  离散傅立叶变换(Discrete Fourier Transfor)           |
+-----------------+------------------------------------------------------+

下图为四种信号图例：

.. image:: /images/2015-07-13.png
    :alt: fouries-category

在计算机上能适用的只有离散傅立叶变换！


参考
===========

#. `The Scientist and Engineer's Guide to Digital Signal Processing <http://www.dspguide.com/pdfbook.htm>`_
#. http://www.fourier-series.com/

