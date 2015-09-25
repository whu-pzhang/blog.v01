Linux下安装GSL
################

:author: pzhang
:date: 2015-09-24
:modified: 
:category: Programming
:tags: C, GSl
:slug: install-gsl-under-linux

GSL全称为GNU Scientific Library，是一个用C语言写的数值计算库。

其包含的数值计算库包罗万象，非常丰富。

开源协议为 GNU General Public License

.. contents::

获取GSL
=========

GSL 源码可以通过多种方式获取，其官方站点为

http://www.gnu.org/software/gsl/

安装
=========

下载gsl-latest.tar.gz到本地后，先解压：

::

    $ tar zxvf gsl-latest.tar.gz

会得到文件夹 gsl-1.16（本文下载时候的版本）

::

    $ cd gsl-1.16
    $ ./configure
    $ make
    $ make check > log 2>&1  # 检查！ 只有失败的信息会显示
    $ sudo make install

添加链接库
--------------

安装完成后，需要将其库文件夹加入 \ ``LD_LIBRARY_PATH``\，不然执行时会报如下错误：

::

    $ ./a.out
    ./a.out: error while loading shared libraries:
    libgsl.so.0: cannot open shared object file: No such
    file or directory

添加：

.. code-block:: bash

    $ echo "LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:/usr/local/lib" >> ~/.bashrc
    $ echo "export LD_LIBRARY_PATH" >> ~/.bashrc
    $ exex $SHELL

使用
==========

默认安装在\ ``/usr/local/``\下了。

编译
-----

头文件包含在 \ ``/usr/local/include/gsl/``\下， 常规的编译命令如下

::

    $ gcc -Wall -I/usr/local/include -c example.c

编译完成得到目标文件 example.o

gcc 默认的 include 搜索路径就包含有 \ ``/usr/local/include``\，因此GSL默认安装时，编译的-I选项是可以省略的

链接
------

gsl 安装得到的是一个单一库文件 \ ``libgsl.a``\，shared libraray \ ``libgsl.so``\ 同样也安装了。默认位置为 \ ``/usr/local/lib``\

链接库时，不仅需要指定主要库，还要指定一个提供基础线性代数运算支持的 \ ``CBLAS``\库，其库文件名为 \ ``libgslcblas.a``\

链接命令如下：

::

    $ gcc -L/usr/local/lib example.o  -lgsl -lgslcblas -lm

同编译，因为默认就包含在gcc搜索路径只中，这里的-L也可以省略。

实例
=======

计算x=5时的贝塞尔函数\ :math:`J_0(x)`\的值

.. code-block:: C

    #include <stdio.h>
    #include <gsl/gsl_sf_bessel.h>

    int main(void)
    {
        double x = 5.0;
        double y = gsl_sf_bessel_J0(x);

        printf("J0(%g) = %.18e\n", x, y);
        return 0;
    }

编译链接：

::

    $ gcc example.c -lgsl -lgslcblas -lm -Wall

运行：

::

    $ ./a.out
    J0(5) = -1.775967713143382642e-01

参考
=======

#. `GSL Manual <https://www.gnu.org/software/gsl/manual/>`_

