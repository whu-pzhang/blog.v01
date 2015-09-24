快速傅里叶变换
################

:date: 2015-08-10
:author: pzhang
:tags: 
:slug: fast-fourier-transform

.. contents::

基2的快速傅立叶算法
====================

数学推导
------------

我们知道N点的离散傅里叶变换公式如下:

.. math::
    X[k] = \sum_{n=0}^{N-1} {x[n] e^{-j2\pi nk /N}}

首先，将其按序号分解为偶数和奇数两部分:

.. math::
    X[k] = \sum_{n=0}^{(N/2)-1} {x[2n] e^{-j2\pi(2n)k/N}} + \sum_{n=0}^{(N/2)-1} {x[2n+1] e^{-j2\pi (2n+1)k/N}}

将第二项中固定相位角的部分提出，化简为：

.. math::
    X[k] = \sum_{n=0}^{(N/2)-1} {x[2n] e^{-j2\pi(2n)k/N}} + e^{-j2\pi k/N} \sum_{n=0}^{(N/2)-1} {x[2n+1] e^{-j2\pi (2n)k/N}}

为了简化式子，令\ :math:`W_N = e^{j2\pi/N}`\，则上式可写为：

.. math::
    X[k] = \sum_{n=0}^{(N/2)-1} {x[2n] W_N^{2nk}} + W_N^k \sum_{n=0}^{(N/2)-1} {x[2n+1] W_N^{2nk}}

由\ :math:`W_N^2 = e^{-j2\pi 2/N} = e^{-j2\pi/(N/2)}`\，可将\ :math:`W_N^2`\由\ :math:`W_{N/2}`\代替，这样上式化为：

.. math::
    X[k] = \sum_{n=0}^{(N/2)-1} {x[2n] W_{N/2}^{nk}} + W_N^k \sum_{n=0}^{(N/2)-1} {x[2n+1] W_{N/2}^{nk}}

这样我们就得到含有 N/2 的两项，相加即得到N点的DFT.

考虑输出 \ :math:`X[k+N/2]`\：

.. math::
    X[k+N/2] = \sum_{n=0}^{(N/2)-1} {x[2n] W_{N/2}^{n(k+N/2)}} + W_N^{(k+N/2)} \sum_{n=0}^{(N/2)-1} {x[2n+1] W_{N/2}^{n(k+N/2)}}

其中，\ :math:`W_{N/2}^{n(k+N/2)}`\可以化简：

.. math::
    W_{N/2}^{n(k+N/2)} &= W_{N/2}^{nk} W_{N/2}^{nN/2} = W_{N/2}^{nk} e^{-j2\pi n} \\
                       &= W_{N/2}^{nk}

而对于\ :math:`W_N^{k+N/2}`\也可以化简为：

.. math::
    W_N^{k+N/2} = W_N^k W_N^{N/2} = W_N^k e^{-j2\pi N/2N} = -W_N^k

将这两项代入前面的\ :math:`X[k+N/2]`\中，再与\ :math:`X[m]`\的式子联立即得到：

.. math::
    X[k] &= \sum_{n=0}^{(N/2)-1} {x[2n] W_{N/2}^{nk}} + W_N^k \sum_{n=0}^{(N/2)-1} {x[2n+1] W_{N/2}^{nk}} \\
    X[k+N/2] &= \sum_{n=0}^{(N/2)-1} {x[2n] W_{N/2}^{nk}} - W_N^k \sum_{n=0}^{(N/2)-1} {x[2n+1] W_{N/2}^{nk}}




代码实现
----------
