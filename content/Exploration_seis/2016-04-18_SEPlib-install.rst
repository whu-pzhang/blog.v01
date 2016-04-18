SEPlib安装
##############

:date: 2016-04-18
:author: pzhang
:category: SEPlib
:tags: SEPlib, 安装
:slug: SEPlib-install

.. contents::

很早之前就想安装SEPlib了，一直没成功。


依赖
================

- 编译器 intel Fortran complier
- flex
- fftw3
- lesstif (or openmotif)

安装依赖包::

    $ sudo yum install fftw3 fftw3-devel
    $ sudo yum install flex flex-devel
    $ sudo yum install lesstif lesstif-devel

环境变量::

    export FC=ifort
    export FFTW_FCLD="-L/usr/lib64 -lfftw3f"

安装
================

进入安装目录::

    ./configure --prefix=/opt/seplib --with-su=/home/pzhang/cwp --with-fftw
    make

这时会报错，

.. code-block:: c

    super_chain_mod.f90(79): error #7061: The characteristics of dummy argument 1 of the associated actual procedure differ from the characteristics of dummy argument 1 of the dummy procedure.   [FLAT_CHAIN_ADJ_EXEC]
    call chain20( flat_chain_adj_exec,flat_chain_exec,adj, add, model, data,temp2)
    ------------------^

经过Google后发现是由于在调用 ``chain20`` 这个函数时，传入的函数类型和声明的不匹配。

我们需要修改 ``seplib-6.5.3/seplib_base/lib/seis/dip/super_chain_mod.f90``
文件。

修改第50行，将

.. code-block:: fortran

    logical       :: adj, add,adj_new

改为

.. code-block:: fortran

    logical,intent(in)  :: adj, add
    logical             :: adj_new

其他出错的地方作类型的修改。总共需要修改5处！

编译全部通过之后，::

    $ sudo make install

然后添加环境变量即可

.. code-block:: bash

    export SEPHOME=/opt/seplib
    export SEPINC=$SEPHOME/include    
    export PATH=${PATH}:$SEPHOME/bin
    export DATAPATH=$HOME/SEPData

    


参考
===============

#. https://software.intel.com/en-us/forums/intel-fortran-compiler-for-linux-and-mac-os-x/topic/472080
#. http://www.linuxidc.com/Linux/2010-01/23959.htm




