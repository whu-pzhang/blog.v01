MPI学习之入门
################

:date: 2016-04-15
:author: pzhang
:category: Programming
:tags: C, MPI
:slug: MPI-getstarted

.. contents::

MPI并行编程是科学计算加速的重要手段。

例子
============

计算1到10^8的累加和，分别用串行和MPI并行实现,比较两者之间的速度。

直接看例子：

.. code-block:: c

    #include <stdio.h>
    #include <time.h>
    #ifdef MPI
    #include <mpi.h>
    #define MASTER 0    // 主进程
    #endif
    
    #define N 100000000 // Array size
    
    int main(int argc, char *argv[])
    {
        long a[N], i;
        long allSum = 0;
    
        for (i=0; i<N; i++) {
            a[i] = i+1;
        }
    #ifndef MPI
        struct timespec now, tmstart;
        clock_gettime(CLOCK_REALTIME, &tmstart);
        for (i=0; i<N; i++) {
            allSum += a[i];
        }
        clock_gettime(CLOCK_REALTIME, &now);
        double wtime = (double)((now.tv_sec+now.tv_nsec*1e-9)-(double)(tmstart.tv_sec+tmstart.tv_nsec*1e-9));
        printf("The sum(Ser) is %ld. Token %f secs\n", allSum, wtime);
    #else
        /*********************************/
        MPI_Init(NULL, NULL);
        int world_rank; // 当前进程号
        MPI_Comm_rank(MPI_COMM_WORLD, &world_rank);
        int world_size; // 总的进程数
        MPI_Comm_size(MPI_COMM_WORLD, &world_size);
    
        int subdomainStart = N/world_size * world_rank;
        int subdomainSize = N/world_size;
        // 不能等分时，将不规则的块交给最后一个进程处理
        if (world_rank == world_size-1) {
            subdomainSize += N%world_size;
        }
        int subdomainEnd = subdomainStart+subdomainSize;
    
        long localSum;
    
        double start = MPI_Wtime();
        if (world_rank != MASTER) {
            localSum = 0;
            for (i=subdomainStart; i<subdomainEnd; i++) {
                localSum += a[i];
            }
            MPI_Send(&localSum, 1, MPI_LONG, 0, 99, MPI_COMM_WORLD);
            printf("Process %d of %d. sum = %ld\n", world_rank, world_size, localSum);
        } else {
            for (i=subdomainStart; i<subdomainEnd; i++) {
                allSum += a[i];
            }
            printf("Process %d of %d. sum = %ld\n", world_rank, world_size, allSum);
            for (i=1; i<world_size; i++) {
                MPI_Recv(&localSum, 1, MPI_LONG, i, 99, MPI_COMM_WORLD, MPI_STATUS_IGNORE);
                allSum += localSum;
            }
        }
        double finish = MPI_Wtime();
    
        if (world_rank == MASTER) {
            printf("\nThe sum(MPI) is %ld. Token %f secs\n", allSum, finish-start);
        }
        MPI_Finalize();
    #endif
    
        return 0;
    }

该程序为计算1到10^8的累加和。首先看串行计算的速度::

    $ gcc -g -Wall -o sum mpi_sum.c
    $ ./sum
    The sum(Ser) is 5000000050000000. Token 0.201456 secs

在来看看MPI并行的速度::

    $ mpicc -g -Wall -o mpi_sum mpi_sum.c -DMPI
    $ mpirun -n 2 ./mpi_sum     # 利用2个进程
    Process 1 of 2. sum = 3750000025000000
    Process 0 of 2. sum = 1250000025000000

    The sum(MPI) is 5000000050000000. Token 0.105313 secs
    $ mpirun -n 4 ./mpi_sum     # 4个进程
    Process 0 of 4. sum = 312500012500000
    Process 2 of 4. sum = 1562500012500000
    Process 3 of 4. sum = 2187500012500000
    Process 1 of 4. sum = 937500012500000

    The sum(MPI) is 5000000050000000. Token 0.056803 secs

可以看到MPI程序加速明显，利用加速比和效率计算公式：

.. math::
    S &= \frac{T_{serial}}{T_{mpi}} \\
    E &= \frac{T_{serial}}{p \times T_{mpi}}

可以算得2个和4个进程下的加速比和效率分别为：

.. math::
    S_2 = 1.91293 \qquad  E_2 = 0.956463 \\
    S_4 = 3.54657 \qquad  E_4 = 0.886643

加速比虽然会随着进程数的增加而增加，但并不是线性的。而效率则会随着进程数的增加而降低。

该例子时通过MPI最基础的点对点(point-to-point)通讯方式来实现的，事实上利用MPI提供的

.. code-block:: c

    int MPI_Reduce(
            void*           input_data_p    /* in */,
            void*           out_data_p      /* out */,
            int             count           /* in */,
            MPI_Datatype    datatype        /* in */,
            MPI_Op          operator        /* in */,
            int             dest_process    /* in */,
            MPI_Comm        comm            /* in */);

函数可以更简便的实现。

参考
============

#. `MPI Tutorial <http://mpitutorial.com/>`_

