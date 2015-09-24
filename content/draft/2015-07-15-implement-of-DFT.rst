离散傅立叶变换的代码实现
#########################

:date: 2015-07-15
:author: pzhang
:category: Basic_Seis, Programming
:tag: 傅里叶变换
:slug: implement-of-DFT

.. contents::


前面文章里对傅里叶变换的理论进行了复习。这篇文章主要是学些在代码上怎么实现离散傅立叶变换。


DFT
==========

离散形式的傅里叶变换可以写为：

.. math::
    X_k = \sum_{n=0}^{N-1} {x_n \cdot e^{-i 2\pi k n / N} }

逆变换为：

.. math::
    x_n = \frac {1}{N} \sum_{k=0}^{N-1} {X_k \cdot e^{i 2\pi k n / N} }

只看正变换，我们可以发现其只是一个简单的矩阵乘法：

.. math::
    \vec{X} = M \cdot \vec{x}

矩阵\ :math:`M`\表示为：

.. math::
    M_{kn} = e^{-i 2\pi k n / N}

那么我们就可以用矩阵乘法来计算DFT：

.. code-block:: python

    import numpy as np

    def DFT_slow(x):
        """Calculate the Discrete Fourier Transform of the 1D real array x"""
        x = np.asarray(x, dtype=float) # 将一维列表转换成numpy中的array
        N = x.shape[0]
        n = np.arange(N)
        k = n.reshape((N, 1))
        F = np.exp(-2j * np.pi * k * n / N )
        return np.dot(F, x)

来验证一下正确与否：

.. code-block:: python
    
    x = np.random.random(1024)
    np.allclose(DFT_slow(x), np.fft.fft(x))

返回::
    
    True

效率比较：

.. code-block:: python

    %timeit DFT_slow(x)
    %timeit np.fft.fft(x)

结果::

    10 loops, best of 3: 18.9 ms per loop
    100000 loops, best of 3: 8.24 µs per loop

可以看到，Python 写的DFT与numpy库的快速傅立叶算法运算效率差了4个数量级！

对于长度为 N 的输入，我们实现的算法复杂度是 \ :math:`O(N^2)`\ 级的，而快速傅立叶算法（FFT）的则为
\ :math:`O(N log_2N)`\。

FFT
=============

FFT算法效率高的原因在于其利用了对称性和奇偶性。



