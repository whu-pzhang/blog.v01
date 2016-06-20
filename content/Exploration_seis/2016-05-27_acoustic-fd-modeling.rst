时间域声波二维有限差分基础
################################

:author: pzhang
:date: 2016-05-27
:category: 
:tags: Madagacar, 有限差分,
:slug: acoustic-fd-modeling
:mathjax: true

.. contents::

在模拟地震波传播的方法中，有限差分(Finite-Difference)方法占有主导地位，并且在
地震勘探工业界以及结构模拟中扮演者越来越重要的角色。这是因为FD方法能够在较为复杂
的模型中 提供波场传播相对精确的“完全”解，与此同时，其计算效率也相对较高，并且比较
容易并行化。

本文介绍二维声波有限差分的原理。

二维声波方程
================================

声波方程表示如下：

.. math::
    \Delta U - \frac{1}{v^2} \frac{\partial^2 U}{\partial^2 t} = f(t)

变换一下写为：

.. math::
    | \Delta - f(t) | v^2 = \frac{\partial^2 U}{\partial t^2}

其中\ :math:`\Delta`\为Laplacian算符， \ :math:`f(t)`\为震源函数，\ :math:`v`\为
速度，\ :math:`U`\为标量波场。

为了在计算机上实现，我们需要将该式子在时间和空间上离散化。具体实现就是利用Taylor级数
展开。本文只考虑时间二阶空间四阶的差分方程。

时间上的离散
------------------

为了将\ :math:`{\partial^2 U} / {\partial t^2}`\离散化。首先将位移
\ :math:`U_{i+1}`\在时间步长上进行Taylor展开:

.. math::
    U_{i+1} = U_i + \frac{\partial U}{\partial t} \Delta t +
    \frac{1}{2} \frac{\partial^2 U}{\partial^2 t} (\Delta{t})^2 +
    \frac{1}{6} \frac{\partial^3 U}{\partial^3 t} (\Delta{t})^3 + {higher\ order\ terms}

其中\ :math:`U_i`\表示的不同时间的波场。
从中解出\ :math:`\partial{U}/ \partial{t}`\得到：

.. math::
    \frac{\partial U}{\partial t} = \frac{1}{\Delta t} \big( U_{i+1} - U_i \big) - 
    \frac{1}{2} \frac{\partial^2 U}{\partial^2 t} \Delta{t} -
    \frac{1}{6} \frac{\partial^3 U}{\partial^3 t} (\Delta{t})^2 - ...

对其作近似得到：

.. math::
    \frac{\partial U}{\partial t} \approx \frac{1}{\Delta{t}} \big( U_{i+1} - U_i \big)

那么该近似的截断误差正比于\ :math:`\Delta t`\。

为了获得更好的近似，考虑\ :math:`U_{i-1}`\的Taylor展开式：

.. math::
    U_{i-1} = U_i - \frac{\partial U}{\partial t} \Delta t +
    \frac{1}{2} \frac{\partial^2 U}{\partial^2 t} (\Delta{t})^2 -
    \frac{1}{6} \frac{\partial^3 U}{\partial^3 t} (\Delta{t})^3 + {higher\ order\ terms}

同样解出\ :math:`\partial{U}/ \partial{t}`\得到：

.. math::
    \frac{\partial U}{\partial t} = \frac{1}{\Delta t} \big( U_{i} - U_{i-1} \big) + 
    \frac{1}{2} \frac{\partial^2 U}{\partial^2 t} \Delta{t} -
    \frac{1}{6} \frac{\partial^3 U}{\partial^3 t} (\Delta{t})^2 - ...

将该式子与前面得到的\ :math:`\partial{U}/ \partial{t}`\联合起来得到：

.. math::
    \frac{\partial U}{\partial t} = \frac{1}{2\Delta{t}} \big( U_{i+1} - U_{i-1} \big)
    - \frac{1}{6} \frac{\partial^3 U}{\partial^3 t} (\Delta{t})^2 - ...

近似得到：

.. math::
    \frac{\partial U}{\partial t} = \frac{1}{2\Delta{t}} \big( U_{i+1} - U_{i-1} \big)

可以看到现在的误差是正比于\ :math:`（\Delta{t})^2`\，比前面的近似误差小了。

同样的，我们将前面两个Taylor展开式相加可以得到位移对时间的二阶偏导近似表达式：

.. math::
    \frac{\partial^2{U}}{\partial{t^2}} \approx \frac{1}{(\Delta{t})^2} \big( U_{i+1}+U_{i-1}-2U_{i} \big)

有了这个式子，我们就可以将开始的波动方程在时间上离散化，将其表示而二阶时间精度的差分形式：

.. math::
    U_{i+1} = \big[ \Delta{U} - f(t) \big] v^2 (\Delta{t})^2 - 2U_{i} - U_{i-1}


空间上的离散
---------------------

有了前面时间上离散作参考，空间上的离散其实是一样的，只不过我们会考虑更高阶次的近似。

同样的，将位移\ :math:`U_{i+1}`\和\ :math:`U_{i-1}`\对\ :math:`x`\方向作Taylor展开：

.. math::
    U_{i+1} = U_i + \frac{\partial U}{\partial x} \Delta{x} +
    \frac{1}{2} \frac{\partial^2 U}{\partial x^2} (\Delta{x})^2 +
    \frac{1}{6} \frac{\partial^3 U}{\partial x^3} (\Delta{x})^3 + 
    \frac{1}{24} \frac{\partial^4 U}{\partial x^4} (\Delta{x})^4 +
    \frac{1}{120} \frac{\partial^5 U}{\partial^5 x} (\Delta{x})^5 + {higher\ order\ terms} \\

    U_{i-1} = U_i - \frac{\partial U}{\partial x} \Delta{x} +
    \frac{1}{2} \frac{\partial^2 U}{\partial x^2} (\Delta{x})^2 -
    \frac{1}{6} \frac{\partial^3 U}{\partial x^3} (\Delta{x})^3 + 
    \frac{1}{24} \frac{\partial^4 U}{\partial x^4} (\Delta{x})^4 -
    \frac{1}{120} \frac{\partial^5 U}{\partial^5 x} (\Delta{x})^5 + {higher\ order\ terms} \\

    U_{i+2} = U_{i} + \frac{\partial U}{\partial x} (2\Delta{x}) +
    \frac{1}{2} \frac{\partial^2 U}{\partial x^2} (2\Delta{x})^2 +
    \frac{1}{6} \frac{\partial^3 U}{\partial x^3} (2\Delta{x})^3 + 
    \frac{1}{24} \frac{\partial^4 U}{\partial x^4} (2\Delta{x})^4 +
    \frac{1}{120} \frac{\partial^5 U}{\partial x^5} (2\Delta{x})^5 + {higher\ order\ terms} \\

    U_{i-2} = U_{i} - \frac{\partial U}{\partial x} (2\Delta{x}) +
    \frac{1}{2} \frac{\partial^2 U}{\partial x^2} (2\Delta{x})^2 -
    \frac{1}{6} \frac{\partial^3 U}{\partial x^3} (2\Delta{x})^3 + 
    \frac{1}{24} \frac{\partial^4 U}{\partial x^4} (2\Delta{x})^4 -
    \frac{1}{120} \frac{\partial^5 U}{\partial x^5} (2\Delta{x})^5 + {higher\ order\ terms}

将上述四个式子两两相加再相减略去五阶以上的高阶项并消去四阶项可以得到：

.. math::
    \frac{\partial^2 U}{\partial x^2} = \frac{1}{(\Delta{x})^2} \big[  -\frac{1}{12} U_{i+2} + \frac{16}{12} U_{i+1} -
    \frac{30}{12} U_{i} +\frac{16}{12} U_{i-1} - \frac{1}{12} U_{i-2} \big]

同理可以得到关于z的空间四阶差分格式如下：

.. math::
    \frac{\partial^2 U}{\partial z^2} = \frac{1}{(\Delta{z})^2} \big[  -\frac{1}{12} U_{i+2} + \frac{16}{12} U_{i+1} -
    \frac{30}{12} U_{i} +\frac{16}{12} U_{i-1} - \frac{1}{12} U_{i-2} \big]

完整的差分格式
------------------

分别把位移对时间和空间x及z方向做了离散化之后，我们就可以得到Laplacian算符的差分形式：

.. math::
    \Delta{U} =& \frac{\partial^2 U}{\partial x^2} + \frac{\partial^2 U}{\partial z^2} \\
              =& -\frac{1}{12 (\Delta{x})^2} \big( U_{m+2,n}^j + U_{m-2,n}^j \big) +
                 \frac{16}{12 (\Delta{x})^2} \big( U_{m+1,n}^j + U_{m-1,n}^j \big) -
                 \frac{30}{12 (\Delta{x})^2} \big( U_{m,n}^j \big) \\ 
               & -\frac{1}{12 (\Delta{z})^2} \big( U_{m,n+2}^j + U_{m,n-2}^j \big) +
                 \frac{16}{12 (\Delta{z}2)^2} \big( U_{m,n+1}^j + U_{m,n-1}^j \big) -
                 \frac{30}{12 (\Delta{z})^2} \big( U_{m,n}^j \big)

整理一下：

.. math::
    \Delta{U} =& -\frac{30}{12} * U_{m,n}^j * \big( \frac{1}{(\Delta{x})^2} + \frac{1}{(\Delta{x})^2} \big)\\
               & +\frac{16}{12} * \big( U_{m+1,n}^j + U_{m-1,n}^j \big) * \frac{1}{(\Delta{x})^2} \\
               & -\frac{1}{12} * \big( U_{m+2,n}^j + U_{m-2,n}^j \big) * \frac{1}{(\Delta{x})^2} \\
               & +\frac{16}{12} * \big( U_{m,n+1}^j + U_{m,n-1}^j \big) * \frac{1}{(\Delta{z})^2} \\
               & -\frac{1}{12} * \big( U_{m,n+2}^j + U_{m,n-2}^j \big) * \frac{1}{(\Delta{z})^2} \\

其中\ :math:`j`\为时间上网格，\ :math:`m`\和\ :math:`n`\分别为x方向和z方向上的网格。
时间域二阶空间域四阶的波动
方程有限差分格式如下：

.. math::
    U_{m,n}^{j+1} = 

