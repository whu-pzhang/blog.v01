一些有用的算法收集
######################

:author: pzhang
:date: 2015-10-16
:modified: 2016-04-14
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


产生一定范围内的随机数
======================

最显然的办法就是::
    
    rand() % N

产生0到N-1之间的随机数，但是很多随机数生成器在数字低位上并不是随机的。更好的办法是::

    (int)((double)rand() / ((double)RAND_MAX + 1) * N)

看例子

.. code-block:: c

    #include <stdio.h>
    #include <stdlib.h>
    #include <time.h>
    #define N 10

    int main(int argc, char const *argv[])
    {
        int a[N], b[N];
        float c[N], d[N];

        srand(time(NULL));
        for (int i=0; i<N; i++) {
            a[i] = rand() % N;  // POOR!
            b[i] = (int)((double)rand() / ((double)RAND_MAX + 1) * N);  // 产生0~N-1之间的整数
            c[i] = (float)rand() / ((float)RAND_MAX / N + 1);   // 产生0~N-1之间的浮点数

            printf("a[%d] = %d; b[%d] = %d; c[%d] = %f\n",
                i, a[i], i, b[i], i, c[i]);
        }
        return 0;
    }

输出::

    $ ./rand_test 
    a[0] = 9; b[0] = 3; c[0] = 4.707045
    a[1] = 4; b[1] = 5; c[1] = 1.947819
    a[2] = 9; b[2] = 3; c[2] = 8.725301
    a[3] = 1; b[3] = 6; c[3] = 2.441400
    a[4] = 7; b[4] = 0; c[4] = 7.176581
    a[5] = 1; b[5] = 2; c[5] = 5.303668
    a[6] = 6; b[6] = 2; c[6] = 3.098936
    a[7] = 0; b[7] = 3; c[7] = 5.129158
    a[8] = 5; b[8] = 9; c[8] = 3.879813
    a[9] = 3; b[9] = 6; c[9] = 7.315299

总结下来，要产生[M, N]之间的随机数，可以按照以下的公式::

    M + rand()/(RAND_MAX / (N-M+1) + 1)



参考
========

#. `How to tell if a Linux system is big endian or little endian? <http://serverfault.com/questions/163487/how-to-tell-if-a-linux-system-is-big-endian-or-little-endian>`_
#. `Ten Ways to Check if an Integer Is a Power Of Two in C <http://www.exploringbinary.com/ten-ways-to-check-if-an-integer-is-a-power-of-two-in-c/>`_

#. http://c-faq.com/lib/randrange.html



修改
=======

- 2015-10-16： 初稿
- 2016-04-14： 增加随机数部分

