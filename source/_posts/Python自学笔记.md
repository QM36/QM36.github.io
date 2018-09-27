---
title: Python自学笔记
---

### 一、迭代器
1. 迭代器是访问集合元素的一种方式，从集合的第一个元素开始访问，直到所有元素得访问结束，迭代器只能往前不能后退。
2. 迭代器有两种基本方法：iter()和next()
3. 字符串列表或者元组都可以用于创建迭代器
```python
   list = [1,2,3,4]
   it = iter(list)
   print(next(it))
   #结果为1
   print(nexy(it))
   #结果为2
```
4. 迭代器可以代替for
```python
   list = [1,2,3,4]
   it = iter(list)
   while Tssrue:
	   print (next(it))
	   pass

```
### 二、生成器
1. 使用了 yield 的函数被称为生成器（generator）
2. 生成器的返回值就是一个迭代器
### 三、函数
#### 1、调用函数
1. 函数调用注意参数个数：abs()只能有一个参数，max()可以有多个参数，int()为类型转换函数
2. 函数名就是指向一个对象的引用，可以把函数名赋给一个变量，相当于别名绑定
```python
a = abs
a(-1)
#输出1
```
#### 2. 定义函数
1. 定义函数用def,结构如下：def function_name():
2. 函数的返回值：遇到return 函数执行完毕，并且返回最终结果，如果没有返回值，可以简写为return
3. 在Python交互环境中定义函数时，注意Python会出现...的提示。函数定义结束后需要按两次回车重新回到>>>提示符下
4. 如果已经把my_abs()的函数定义保存为abstest.py文件了，那么，可以在该文件的当前目录下启动Python解释器，用from abstest import my_abs来导入my_abs()函数，注意abstest是文件名（不含.py扩展名）
5. 函数的定义需要有类型的检查，用内置函数isinstance()实现
6. 函数可以返回多个值，其实就是一个元组
```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```
### 问题
1. 迭代器就是用于循环吗？for in 不能完成吗？
2. 没有理解生成器### 一、迭代器
1. 迭代器是访问集合元素的一种方式，从集合的第一个元素开始访问，直到所有元素得访问结束，迭代器只能往前不能后退。
2. 迭代器有两种基本方法：iter()和next()
3. 字符串列表或者元组都可以用于创建迭代器
```python
   list = [1,2,3,4]
   it = iter(list)
   print(next(it))
   #结果为1
   print(nexy(it))
   #结果为2
```
4. 迭代器可以代替for
```python
   list = [1,2,3,4]
   it = iter(list)
   while Tssrue:
     print (next(it))
     pass

```
### 二、生成器
1. 使用了 yield 的函数被称为生成器（generator）
2. 生成器的返回值就是一个迭代器
### 三、函数
#### 1、调用函数
1. 函数调用注意参数个数：abs()只能有一个参数，max()可以有多个参数，int()为类型转换函数
2. 函数名就是指向一个对象的引用，可以把函数名赋给一个变量，相当于别名绑定
```python
a = abs
a(-1)
#输出1
```
#### 2. 定义函数
1. 定义函数用def,结构如下：def function_name():
2. 函数的返回值：遇到return 函数执行完毕，并且返回最终结果，如果没有返回值，可以简写为return
3. 在Python交互环境中定义函数时，注意Python会出现...的提示。函数定义结束后需要按两次回车重新回到>>>提示符下
4. 如果已经把my_abs()的函数定义保存为abstest.py文件了，那么，可以在该文件的当前目录下启动Python解释器，用from abstest import my_abs来导入my_abs()函数，注意abstest是文件名（不含.py扩展名）
5. 函数的定义需要有类型的检查，用内置函数isinstance()实现
6. 函数可以返回多个值，其实就是一个元组
```python
def my_abs(x):
    if not isinstance(x, (int, float)):
        raise TypeError('bad operand type')
    if x >= 0:
        return x
    else:
        return -x
```
### 问题
1. 迭代器就是用于循环吗？for in 不能完成吗？
2. 没有理解生成器