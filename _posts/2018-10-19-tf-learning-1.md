---
layout: post
title: 'TensorFlow学习笔记（一）'
subtitle: '入坑'
date: 2018-10-13
categories: 笔记
tags: TensorFlow
---

#### 1、低级别的API [\*](https://www.tensorflow.org/programmers_guide/low_level_intro?hl=zh-cn)



-  **<span style="color:#df732c">张量 Tensor</span>** [\*](https://zh.wikipedia.org/wiki/%E5%BC%B5%E9%87%8F)
    1. Tensor是一个**数据单元**的通用术语，可看作向量和矩阵的扩展，是一种多 `rank` 的数据单元
    2. 标量可以视为0阶的tensor，向量为1阶tensor，矩阵为2阶tensor，这里的阶，也就是rank，也称为秩
    3. 一般的，我们可以称一个张量为**rank N tensor**
    4. TensorFlow中使用 `tf.Tensor`作为操作和传递的主要对象

```
3. # a rank 0 tensor; a scalar with shape [],
[1., 2., 3.] # a rank 1 tensor; a vector with shape [3]
[[1., 2., 3.], [4., 5., 6.]] # a rank 2 tensor; a matrix with shape [2, 3]
[[[1., 2., 3.]], [[7., 8., 9.]]] # a rank 3 tensor with shape [2, 1, 3]
```

- **<span style="color:#df732c">变量 Variable</span>**
    1. tf中，`tf.Variable`是可操作变量，表示可改变值的张量
    2. 在tf内部，`tf.Variable`会被持久性储存，允许被多个Session访问

- **<span style="color:#df732c">图 Graph</span>**
    1. TensorFlow用数据流图来表示计算任务，图中的节点被称为op，即operation，一个操作节点的基类为 `tf.Operation`
    2. `tf.Graph`是流程图类，包含图结构和图集合。
    3. 大多数程序仅依赖于默认图

- **<span style="color:#df732c">会话 Session</span>**
    1. TensorFlow使用`tf.Session`类来表示客户端程序
    2. 在**with**代码块中，退出代码块后`tf.Session`会被关闭释放，如果不使用**with**代码块，那么就需要调用`tf.Session.close`来释放资源
    3. 会话的初始化：`tf.Session.init`

```python
x = tf.constant([[37.0, -23.0], [1.0, 4.0]])
w = tf.Variable(tf.random_uniform([2, 2]))
y = tf.matmul(x, w)
output = tf.nn.softmax(y)
init_op = w.initializer

with tf.Session() as sess:
  # Run the initializer on `w`.
  sess.run(init_op)

  # Evaluate `output`. `sess.run(output)` will return a NumPy array containing
  # the result of the computation.
  print(sess.run(output))

  # Evaluate `y` and `output`. Note that `y` will only be computed once, and its
  # result used both to return `y_val` and as an input to the `tf.nn.softmax()`
  # op. Both `y_val` and `output_val` will be NumPy arrays.
  y_val, output_val = sess.run([y, output])

```

---

#### 2、构建一个简单的神经网络

- 从外部传入data：
    1. 使用 `tf.placeholder()`作为容器，以 `sess.run(***, feed_dict={input: **})` 的形式传入数据

```python
import tensorflow as tf

#在 Tensorflow 中需要定义 placeholder 的 type ，一般为 float32 形式
input1 = tf.placeholder(tf.float32)
input2 = tf.placeholder(tf.float32)

# mul = multiply 是将input1和input2 做乘法运算，并输出为 output 
ouput = tf.multiply(input1, input2)    

with tf.Session() as sess:
    print(sess.run(ouput, feed_dict={input1: [7.], input2: [2.]}))
# [ 14.]
```

- 添加神经网络层
```python
def add_layer(inputs, in_size, out_size, activation_function=None):
    #1 参数的初始化
    Weights = tf.Variable(tf.random_normal([in_size, out_size]))
    #生成初始参数时，最好用一个随机变量，这里的权值为一个in_size行，out_size列的随机变量矩阵
    Biases = tf.Variable(tf.zeros([1, out_size]) + 0.1)
    #机器学习中，偏置的推荐值不为0，因此这里是在0向量的基础上增加了0.1
    
    #2 神经层的输出
    Wx_plus_b = tf.matmul(inputs, Weights) + Biases
    #定义神经网络的默认值，matmul为矩阵乘法函数
    if activation_function is None:
        outputs = Wx_plus_b
    else:
        outputs = activation_function(Wx_plus_b)
    #如果激活函数为None，那么outputs = Wx_plus_b，否则outputs为激活函数对Wx_plus_b的响应值
    return outputs
```

- 定义学习步骤&初始化参数

```python
train_step = tf.train.GradientDescentOptimizer(0.1).minimize(loss)
#定义tf要如何学习，这里采用了梯度下降法，选取学习效率为0.1，目标为最小化误差loss

init = tf.global_variables_initializer()
#使用变量时，需要对它们进行初始化，这一函数可以一次性初始化所有变量

sess = tf.Session()
sess.run(init)
#定义Session，并进行变量的初始化工作
```