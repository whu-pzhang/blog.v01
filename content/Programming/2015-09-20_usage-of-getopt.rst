getopt用法
#######################

:date: 2015-09-20
:author: pzhang
:modified:
:category: Programming
:tags: Linux, C
:slug: usage-of-getopt

.. contents::

getopt介绍
===============

\ ``getopt``\为Linux下处理命令行参数的函数，相对于自己编写代码来解析命令行参数，
getopt提供了更为方便和友好的方式。

函数\ ``getopt``\包含于系统头文件\ ``unistd.h``\中，其函数原型为:

.. code-block:: C

    int getopt(int argc, char *const argv[], const char *optstring);
    extern char *optarg;
    extern int optind, opterr, optopt;

其中，\ ``argc``\和\ ``argv``\是由\ ``main()``\函数传递的参数个数和内容，\ ``optstring``\代表待处理的选项字符串;

- \ ``optarg``\: 指向当前选项参数的指针
- \ ``optind``\: 再次调用\ ``getopt()``\时，下一个\ ``argv``\指针的索引
- \ ``opterr``\: 非零时，\ ``getopt``\会向标准错误输出错误信息
- \ ``optopt``\: 遭遇未知选项或者遗漏必须参数的选项时，用来存储该选项

\ ``optstring``\中内容的意义（例如\ ``getopt(argc, argv, "ab:c::")``\）

- 单个字符: 表示选项,例如a，b和c
- 单个字符后一个冒号： 表示该选项后必须跟一个参数。参数紧跟在选项后或者以空格隔开，该参数指针赋值给optarg
- 单个字符后两个冒号： 表示该选项后可以跟一个参数也可以不跟。若跟一个参数，则该参数必须紧跟在选项后而不能以空格隔开。

若\ ``getopt``\遇到一个不包括于\ ``argv``\中的选项，或是一个没有参数的选项，将会返回\ ``?``\并将全局变量
\ ``optopt``\设置为该选项字符；若选项首字符为一个冒号\ ``:``\，getopt返回\ ``:``\代替\ ``?``\来表明没有选项参数。


实例说明
=============

getopt 的通常用法如下：

- \ ``getopt``\在循环中调用，当其返回-1时表明没有选项了，循环就此结束
- \ ``switch``\语句用来对getopt返回的值进行分派
- 利用第二个循环来处理余下的非选项参数

多说无益，看实例：

.. code-block:: C

    #include <ctype.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <unistd.h>

    int main(int argc, char *agrv[])
    {
        int aflag = 0;
        int bflag = 0;
        char *cvalue = NULL;
        int index, i;

        opterr = 0;

        while ((c=getopt(argc, argv, "abc:")) != -1)
            switch (c) {
                case 'a':
                    aflag = 1;
                    break;
                case 'b':
                    bflag = 1;
                    break;
                case 'c':
                    cvalue = optarg;
                    break;
                case '?':
                    if (optopt == 'c')
                        fprintf(stderr, "Option -%c requires an argument.\n", optopt);
                    else if (isprint(optopt))
                        fprintf(stderr, "Unknow option -%c.\n", optopt);
                    else
                        fprintf(stderr, "Unknown option character '\\x%x'.\n", optopt); // 以16进制输出未知选项
                        return 1;
                default:
                    abort();
            }
            printf("aflag = %d, bflag = %d, cvalue = %s\n",
                    aflag, bflag, cvalue);

            for (index = optind; index<argc; index++)
                printf("Non-option argument %s\n", argv[index]);

            return 0;
    }

编译链接成可执行文件，运行：

.. code-block:: bash

    $ ./testopt
    aflag = 0, bflag = 0, cvalue = (null)

    $ ./testopt -a -b
    aflag = 1, bflag = 1, cvalue = (null)

    $ ./testopt -ab
    aflag = 1, bflag = 1, cvalue = (null)

    $ ./testopt -c foo
    aflag = 0, bflag = 0, cvalue = foo

    $ ./testopt -cfoo
    aflag = 0, bflag = 0, cvalue = foo

    $ ./testopt arg1
    aflag = 0, bflag = 0, cvalue = (null)
    Non-option argument arg1

    $ ./testopt -a arg1
    aflag = 1, bflag = 0, cvalue = (null)
    Non-option argument arg1

    $ ./testopt -c foo arg1
    aflag = 0, bflag = 0, cvalue = foo
    Non-option argument arg1

    $ ./testopt -a -- -b
    aflag = 1, bflag = 0, cvalue = (null)
    Non-option argument -b

    $ ./testopt -a -
    aflag = 1, bflag = 0, cvalue = (null)
    Non-option argument -

    $ ./testopt -a -我
    Unknown option character '\xffffffce'.



参考
=======

#. `The GNU C Library <http://www.gnu.org/software/libc/manual/html_node/Getopt.html#Getopt>`_
