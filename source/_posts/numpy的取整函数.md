---
title: numpy的取整函数
toc: true
date: 2021-02-09 13:45:35
tags:
- Python
- Numpy
- 取整函数
categories:
- [Python,Numpy]
---

> 前言: 
>
> 最近学习 numpy-100 题, 其中一题涉及一类取整函数, 不同的取整方法会导致不同的取整结果, 这种不同的取整方式对科学计算影响很大, 特此记录.  

<!--more-->

`numpy` 取整函数一共有五个, 分别是 `numpy.ceil`, `numpy.floor`, `numpy.rint` 和 `numpy.trunc`. 接下来一一讲解. 

> 注意: 
>
> 虽然三个函数的作用是取整, 但是返回的结果仍然是 `float` 类型, 只是小数部分变成了零, 数字类型并没有改变. 

# `numpy.ceil`

> 文档描述: 
>
> Return the ceiling of the input, element-wise.
>
> The ceil of the scalar *x* is the smallest integer *i*, such that *i >= x*. It is often denoted as [x]

`numpy.ceil` 是向上取整, 即只要小数位不为零, 都向前进一位. 举例如下: 

```python
>>> a = np.array([-1.7, -1.5, -0.2, 0.2, 1.5, 1.7, 2.0])
>>> np.ceil(a)
array([-1., -1., -0.,  1.,  2.,  2.,  2.])
```

# `numpy.floor`

> 文档描述: 
>
> The floor of the scalar *x* is the largest integer *i*, such that *i <= x*. It is often denoted as [x]

`numpy.floor` 是向下取整, 即只保留该数字的整数部分, 无论小数位有多大. 举例如下: 

```python
>>> a = np.array([-1.7, -1.5, -0.2, 0.2, 1.5, 1.7, 2.0])
>>> np.floor(a)
array([-2., -2., -1.,  0.,  1.,  1.,  2.])
```

# `numpy.trunc`

> 文档描述: 
>
> The truncated value of the scalar *x* is the nearest integer *i* which is closer to zero than *x* is. In short, the fractional part of the signed number *x* is discarded.

`numpy.trunc` 是向接近零的方向取整, 也是不管小数位是否为零. 举例如下: 

```python
>>> a = np.array([-1.7, -1.5, -0.2, 0.2, 1.5, 1.7, 2.0])
>>> np.trunc(a)
array([-1., -1., -0.,  0.,  1.,  1.,  2.])
```

# `numpy.rint`

> 文档描述: 
>
> Round elements of the array to the nearest integer.

`numpy.rint` 就是常说的'四舍五入', 举例如下: 

```python
>>> a = np.array([-1.7, -1.5, -0.2, 0.2, 1.5, 1.7, 2.0])
>>> np.rint(a)
array([-2., -2., -0.,  0.,  2.,  2.,  2.])
```

