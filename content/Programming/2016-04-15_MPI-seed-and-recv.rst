MPI学习之Send和Recv
############################

:date: 2016-04-15
:author: pzhang
:category: Programming
:tags: C, MPI
:slug: MPI-send-and-recv

.. contents::

MPI利用多个进程来计算计算，进程之间的通讯是必不可少的。

本文今天学习了MPI最基本的点对点通讯(point-to-point)。

概述
===========

<<<<<<< HEAD
先看MPI_Send和MPI_Recv的函数原型：

.. code-block::c

    int MPI_Send(
            void*           input_data_p    /* in */,
            int             count           /* in */,
            MPI_Datatype    datatype        /* in */,
            int             dest_process    /* in */,
            int             tag             /* in */,
            MPI_Comm        comm            /* in */);


    int MPI_Recv(
            void*           output_data_p   /* out */,
            int             count           /* in */,
            MPI_Datatype    datatype        /* in */,
            int             src_process     /* in */,
            int             tag             /* in */,
            MPI_Comm        comm            /* in */,
            MPI_Status*     status          /* in */);


MPI基本数据类型
=====================


MPI内部使用自己定义的数据类型，但是大体上和C语言自带的数据类型有着很好的对应
关系。具体见下表

+------------------------+------------------------+
| MPI datatype           | C eqivalent            |
+========================+========================+
| MPI_SHORT              | short int              |
+------------------------+------------------------+
| MPI_INT int            | int                    |
+------------------------+------------------------+
| MPI_LONG               | long int               |
+------------------------+------------------------+
| MPI_LONG_LONG          | long long int          |
+------------------------+------------------------+
| MPI_UNSIGNED_CHAR      | unsigned char          |
+------------------------+------------------------+
| MPI_UNSIGNED_SHORT     | unsigned short int     |
+------------------------+------------------------+
| MPI_UNSIGNED           | unsigned int           |
+------------------------+------------------------+
| MPI_UNSIGNED_LONG      | unsigned long int      |
+------------------------+------------------------+
| MPI_UNSIGNED_LONG_LONG | unsigned long long int |
+------------------------+------------------------+
| MPI_FLOAT              | float                  |
+------------------------+------------------------+
| MPI_DOUBLE             | double                 |
+------------------------+------------------------+
| MPI_LONG_DOUBLE        | long double            |
+------------------------+------------------------+
| MPI_BYTE               | char                   |
+------------------------+------------------------+

除此之外，MPI还支持自定义的数据类型。

例子
==========

先来看一个利用MPI_Send和MPI_Recv进行
=======


>>>>>>> a7a5761affaa2630cc3a914c59619b29a4c2d918
