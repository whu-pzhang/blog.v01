回归和插值
#########################

:author: pzhang
:date: 2015-07-10
:category: Basic_Seis
:tags: math
:slug: regression-and-interpolation
:mathjax: true

.. contents::

最小二乘求回归方程
===================

假定给散点图配置了一条直线\ :math:`y^{'} = a + bx`\ ，则\ :math:`y^{'}`\ 是相应于\ :math:`x_i`\
的理论值，而\ :math:`y_i`\ 为实测值。求解最佳回归方程的原则就是：使得理论值与实测值的残差平方和最小，即

.. math::
    \sum v_{i}^2 = min

也即是

.. math::
    \sum [y_i - (a + b x_i) ]^2 = \sum v_i^2 = min

要满足这个式子，使其对 a 和 b 的微分等于零，求得a和b即可。即

.. math::
    \frac{\partial }{\partial b} {\sum (y_i - a - b x_i)^2}= 0 \\
    \frac{\partial }{\partial a} {\sum (y_i - a - b x_i)^2}= 0

分解后得到：

.. math::
    &\sum {x_i y_i} -b \sum {x_i^2} - a \sum x_i = 0 \\
    &\sum y_i - b \sum x_i - Na = 0

解这个方程组即可得到

.. math::
    &b = \frac {N\sum{x_i y_i}-\sum y_i \sum x_i} {N\sum x_i^2 - {\sum x_i}^2} \\
    &a = \frac {\sum x_i^2 \sum y_i - \sum {x_i y_i} \sum x_i} {N\sum x_i^2 - {\sum x_i}^2}

在实际计算中，求得 b 后， a 值不一定根据上式求得。可以按如下求得

.. math::
    a = \overline{y} - b \overline{x}

此外，对 b 值计算公式进一步推导，可以得到

.. math::
    b = \frac {\sum {(y_i - \overline{y})(x_i - \overline{x})}} {\sum {(x_i - \overline{x})}^2}

相关系数
==========

回归系数 b 为回归方程的斜率，其大小决定了 y 随 x 变化的程度。若没有相关性（零相关），此时 y
不随 x 而变化，b 为零，即：

.. math::
    b = \frac {\sum {(y_i - \overline{y})(x_i - \overline{x})}} {\sum {(x_i - \overline{x})}^2} = 0

如果反过来把 y 看成自变量，x 作为因变量，则回归方程为 \ :math:`x=a^{'} + b^{'}y`\ ，可得：

.. math::
    b = \frac {\sum {(y_i - \overline{y})(x_i - \overline{x})}} {\sum (y_i - \overline{y})^2}

相关关系是“相互”的，若 y 不随 x 变化，则 x 也不随 y 变化，即\ :math:`b^{'} = 0`\，此时可得到x和y
不相关时的情况：

.. math::
    bb^{'} = \frac {[\sum {(y_i - \overline{y})(x_i - \overline{x})}]^2} {\sum (x_i - \overline{x})^2 \sum (y_i - \overline{y})^2 } = 0

完全相关时，该式等于1

因此将这个式子的平方根定义为相关系数 r:

.. math::
    r = \frac {\sum {(y_i - \overline{y})(x_i - \overline{x})}} {\sqrt {\sum (x_i - \overline{x})^2 \sum (y_i - \overline{y})^2 }}

回归直线误差
=============

求出直线回归方程为

.. math::
    y^{'} = a + bx`

其中\ :math:`y^{'}`\为预测值，\ :math:`y`\为观测值。实测值将在这条直线两侧波动，我们用均方误差
的概念来描述这种波动。定义

.. math::
    S_{y^{'}} = \sqrt {\frac {\sum (y_i - y_i^{'})^2} {N-2}}

为剩余均方误差，或叫剩余标准误差。其中 N-2 为自由度，可以这么理解：两个实测点只能做出一条通过
两个点的直线，这时不存在剩余均方误差，只有三个点以上才能作出回归直线。

令 Q 为上式的分子部分，称为剩余平方和或残差平方和。

.. math::
    Q &= \sum (y_i - y_i^{'})^2 \\
      &= \sum [(y_i - \overline{y}) - (y_i^{'} - \overline{y})]^2 \\
      &= \sum (y_i - \overline{y})^2 - 2 \sum {(y_i - \overline{y}) (y_i^{'} - \overline{y})} + \sum (y_i^{'} - \overline{y})^2 \\
      &= \sum (y_i - \overline{y})^2 - 2 \sum {[(y_i - y_i^{'}) + (y_i^{'} - \overline{y})] (y_i^{'} - \overline{y})} + \sum (y_i^{'} - \overline{y})^2 \\
      &= \sum (y_i - \overline{y})^2 - 2 \sum {(y_i - y_i^{'}) (y_i^{'} - \overline{y})} - \sum (y_i^{'} - \overline{y})^2

观测数据较多时，上式右边第二项趋于零，这样 Q 可以表示为

.. math::
    Q = \sum (y_i - \overline{y})^2 - \sum (y_i^{'} - \overline{y})^2

右边第一项为总的平方和，用\ :math:`l_{yy}`\表示，表征了全部观测值总的波动大小；第二项为回归平方和，
用\ :math:`U`\ 表示，反映的是观测值由于回归方程引起的波动部分。

从上式我们可以知道，总的平方和可以分为剩余平方和和回归平方和两部分。由着三种平方和我们可以求得三种均方差，
即总均方差\ :math:`S_y`\、回归均方差\ :math:`S_Q`\和剩余均方差\ :math:`S_{y^{'}}`\：

.. math::
    S_y = \sqrt \frac {l_{yy}}{f_y} , S_Q = \sqrt \frac {Q}{f_Q} , S_{y^{'}} = \sqrt \frac {U}{f_{y^{'}}}

其中 \ :math:`f_y = N-1, f_Q = 1, f_{y^{'}} = N-2`\，分别为各自平方和的自由度。

回归方程的稳定性
=================

回归方程的稳定性就是指除了自变量 x 之外，其他条件基本不变时，不同批次观测数据得到的 a 和 b 的波动情况。
波动越小，回归方程越稳定。



