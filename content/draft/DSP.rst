DSP
#########################

:author: pzhang
:date: 2015-07-12
:category: DSP
:tags: iPython
:slug:


.. code-block:: python

    %pylab inline

.. parsed-literal::

    Populating the interactive namespace from numpy and matplotlib
    

.. code-block:: python

    n = 50
    f = 3
    t = arange(0, 5.01, 0.02)
    x = 0.5 * sin(2*pi*f*t +1)
    plot(t, x)
    xlabel('Time/s')
    ylabel('Amplitude')
    grid(True)


.. image:: /images/output_1_0.png


.. code-block:: python

    N = 200
    dt = 0.02
    f1 = 3
    f2 = 3
    t = arange(0, N*dt + 0.01, dt)
    x1 = 0.5*sin(2*pi*f1*t + 1)
    x2 = 0.3*sin(2*pi*f2*t + 1 + 2*pi)
    subplot(3,1,1)
    plot(t, x1)
    ylim(-1,1)
    title('First')
    subplot(3,1,2)
    plot(t, x2)
    ylim(-1,1)
    title('Sencond')
    subplot(3,1,3)
    plot(t, x1+x2) # 相位相差为2pi整数倍，振幅叠加
    ylim(-1,1)
    title('Added')



.. parsed-literal::

    <matplotlib.text.Text at 0x577c048>




.. image:: /images/output_2_1.png


.. code-block:: python

    %pylab inline

.. parsed-literal::

    Populating the interactive namespace from numpy and matplotlib
    

.. code-block:: python

    N = 1000
    t = linspace(0, 1, N)
    f = 10
    x = sin(2*pi*f*t + 0.3)
    N = 10
    t1 = arange(0, 1.1, 0.1) # arange函数产生的数组不包括终值
    x1 = sin(2*pi*f*t1 + 0.3)
    subplot(3,1,1)
    plot(t,x)
    ylim(-1,1)
    
    subplot(3,1,2)
    plot(t,x,t1,x1,'o')
    ylim(-1,1)
    
    subplot(3,1,3)
    plot(t1,x1)
    ylim(-1,1)



.. parsed-literal::

    (-1, 1)




.. image:: /images/output_4_1.png


.. code-block:: python

    linspace(0, 1, 11)



.. parsed-literal::

    array([ 0. ,  0.1,  0.2,  0.3,  0.4,  0.5,  0.6,  0.7,  0.8,  0.9,  1. ])



