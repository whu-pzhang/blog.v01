有限差分原理及FDELMODC软件使用笔记
#####################################

:author: pzhang
:date: 2015-12-22
:category: 地震学软件
:tags: math
:slug: finite-difference
:mathjax: true

.. contents::

有限差分法作为一种数值模拟方法在各个领域都有广泛地应用。

在我们地震学上，作为一种计算理论地震图的工具使用的尤其广泛。

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





