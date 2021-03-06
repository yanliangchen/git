## 1. 为什么要选择Numpy & Pandas?

两个科学运算当中最为重要的两个模块，一个是 numpy,一个是 pandas。任何关于数据分析的模块都少不了它们两个。

应用

数据分析

机器学习

深度学习

为什么使用 numpy & pandas

运算速度快：numpy 和 pandas 都是采用 C 语言编写, pandas 又是基于 numpy, 是 numpy 的升级版本。

消耗资源少：采用的是矩阵运算，会比 python 自带的字典或者列表快好多



## 2. Numpy的安装

Linux :  

​	$  sudo  pip  install  python-numpy

Windows : 下载numpy的不同版本 下载的路径  给安到指定版本  

​        $  python  -m  pip  install   



## 3. Numpy的学习 

NumPy是Python语言的一个扩充程序库。支持高级大量的维度数组与矩阵运算，此外也针对数组运算提供大量的数学函数库。Numpy内部解除了Python的GIL(全局解释器锁),运算效率极好,是大量机器学习框架的基础库!  

**补充 : GIL全局解释器锁**

 同一进程下的多线程共享数据，共享意味着竞争，竞争带来无序，为了数据安全所以需要加锁进行数据保护，GIL本质是一把互斥锁，使并发变为串行，保证同一时间只有一条线程访问解释器级别的数据，这样就保证了解释器级别的数据安全，同时也带来了一些问题，同一进程只有一条线程执行任务，无法利用多核优势，解决方案可以根据任务的类型来处理，如果是I/O密集型，则需要开多线程提高效率，如果是计算密集型则需要多进程。



**见代码study_numpy.py**

​	

## 4. Pandas的安装

$pip  install  pandas



## 5. Pandas的学习

Pandas是基于Numpy开发出的,专门用于数据分析的开源Python库

**见代码study_pandas.py**