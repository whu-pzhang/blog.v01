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

先看MPI_Send和MPI_Recv的函数原型：

.. code-block:: c

    int MPI_Send(
            void*           msg_buff_p      /* in */,
            int             msg_size        /* in */,
            MPI_Datatype    msg_type        /* in */,
            int             dest_process    /* in */,
            int             tag             /* in */,
            MPI_Comm        comm            /* in */);


    int MPI_Recv(
            void*           msg_buff_p      /* out */,
            int             buf_size        /* in */,
            MPI_Datatype    buf_type        /* in */,
            int             src_process     /* in */,
            int             tag             /* in */,
            MPI_Comm        comm            /* in */,
            MPI_Status*     status_p        /* out */);


MPI基本数据类型
=====================

MPI内部使用自己定义的数据类型，但是大体上和C语言自带的数据类型有着很好的对应
关系。具体见下表

+------------------------+------------------------+
| MPI 数据类型           | C 数据类型             |
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

直接来看利用MPI_Send和MPI_Recv进行通讯的例子：

.. code-block:: c

    #include <stdio.h>
    #include <string.h>
    #include <mpi.h>
    
    #define MASTER 0    // 主进程
    #define MAX_STR 100
    
    int main(void)
    {
        MPI_Init(NULL, NULL);
        int my_rank;
        MPI_Comm_rank(MPI_COMM_WORLD, &my_rank);
        int comm_sz;
        MPI_Comm_size(MPI_COMM_WORLD, &comm_sz);
    
        char hello[MAX_STR];
    
        if (my_rank != MASTER) {
            sprintf(hello, "Hello from process %d of %d.", my_rank, comm_sz);
            MPI_Send(hello, strlen(hello)+1, MPI_CHAR, MASTER, 99,
                MPI_COMM_WORLD);
        } else {
            printf("Hello from process %d of %d.\n", my_rank, comm_sz);
            for (int source=1; source < comm_sz; source++) {
                MPI_Recv(hello, MAX_STR, MPI_CHAR, source, 99,
                    MPI_COMM_WORLD, MPI_STATUS_IGNORE);
                printf("%s\n", hello);
            }
        }
        MPI_Finalize();
        return 0;
    }

对程序编译运行::

    $ mpicc -o send-recv send-recv.c -std=c99 -g -Wall
    $ mpirun -n 4 ./send-recv
    Hello from process 0 of 4.
    Hello from process 1 of 4.
    Hello from process 2 of 4.
    Hello from process 3 of 4.
    

