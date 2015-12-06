一些有用的算法收集
######################

:author: pzhang
:date: 2015-10-16
:category: Programming
:tags: 算法，C
:slug: useful-algorithms

.. contents::

收集了一些可能会用到的小算法。

判断字节序
==========

在处理数据是常常会碰到字节序(Byte Endian)的问题

Linux下可以直接用Shell命令查看：

::

    lscpu | grep -i byte


C语言版
---------

.. code-block:: C

    #include <stdio.h>

    int main(void)
    {
        int test = 1;
        if (*(char *)&test)
            printf("Little endian!\n");
        else
            printf("Big endian!\n");

        return 0;
    }


其原理为：取整形变量test的地址强制转换为字符型指针 \ ``(char *)``\，这个地址
就只指向一个字节的内存块，其为四个字节中的最低地址，最后取这一字节内存块的值。

如果系统字节序为Little endian，则保存1的那一位必定在这一字节中，其值为1；

若为Big endian，则低位取出来的值为0.

bash版
----------

.. code-block:: bash

    echo -n I | od -to2 | head -n1 | cut -f2 -d" " | cut -c6

输出1为Little endian；

0为Big endian.

Python版
-------------

.. code-block:: python

    python -c "import sys;print(0 if sys.byteorder=='big' else 1)"

写成脚本形式可以为：

.. code-block:: python

    #!/usr/bin/env python
    from struct import pack
    if pack('@h', 1) == pack('<h', 1):
        print "Little Endian"
    else:
        print "Big Endian"


判断是否为2的次方
======================

.. code-block:: c

    #include <stdbool.h>  // need c99 support

    bool isPowerOfTwo (unsigned x)
    {
        return !(x & (x-1));
    }

原理：一个数若为2的次方，其二进制形式最高位肯定为1，其余位都为0.那么该数减去1的话，得到的二进制表示则恰恰相反，
最高位为0，其余位为1.这样的话，就可以通过按位与来判断了。


参考
========

#. `How to tell if a Linux system is big endian or little endian? <http://serverfault.com/questions/163487/how-to-tell-if-a-linux-system-is-big-endian-or-little-endian>`_
#. `Ten Ways to Check if an Integer Is a Power Of Two in C <http://www.exploringbinary.com/ten-ways-to-check-if-an-integer-is-a-power-of-two-in-c/>`_



修改
=======

- 2015-10-16： 初稿

