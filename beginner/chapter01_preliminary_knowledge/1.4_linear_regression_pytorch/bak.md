﻿# Pytorch线性回归实战

通过前面的学习我们已经熟知了pytorch的基本用法和自动求导梯度的基本原理，这节课我们将结合前面所学的知识做一个简单的demo，我们将使用pytorch来构建一个简单的线性回归实战案例并且将训练模型的过程进行一次总结

## 1.线性回归模型

首先介绍一下什么是线性回归模型，在初中数学里面我们就接触过线性回归，如在坐标系当中y = ax+b，自变量x与因变量y成线性关系，y随着x的变化而变化则成线性关系。之后我们可以通过x来算出y的预测值。我们这里简单介绍一下一元线性回归模型:利用线性函数(一阶或更低阶多项式)对一个或多个自变量(x或者(x1,x2,...,xk))和因变量(y)之间的关系进行拟合的模型。表达式为:yi = a + b1x1 + b2x2 + ... + bnxn
yi是我们的预测值，我们将目标y与预测值yi进行对比之后求目标函数h(x)，在此我们采用前面所学的梯度下降的方式进行优化。


## 2.基本流程
利用pytorch训练模型的过程大致分为一下五个部分:
1.获取输入数据
如这里我们手动定义一些数据
```x_train = np.array([[3.3], [4.4], [5.5], [6.71], [6.93], [4.168], 
[9.779], [6.182], [7.59], [2.167], [7.042], 
[10.791], [5.313], [7.997], [3.1]], dtype=np.float32) 
y_train = np.array([[1.7], [2.76], [2.09], [3.19], [1.694], [1.573], 
[3.366], [2.596], [2.53], [1.221], [2.827], 
[3.465], [1.65], [2.904], [1.3]], dtype=np.float32)
```
2.定义参数和模型
构建一个未初始化的5\*3的空矩阵（张量）

```
#我们定义好参数w如下将w定义为一个随机的正态分布
W = Variable(torch.randn(1),requires_grad=True)
#b定义成零矩阵:
b = Variable(torch.zeros(1),requires_grad=True)
#定义线性模型:
def linar_model(x):
return w *x +b
```
3.定义损失函数
其中y_预测值,y真实值
```
def get_loss(y_,y):
return torch.mean((y_ - y )**2)```
4.反向传播，得到梯度
```Loss.backward()(反向求梯度)
学习率取0.01
```
5.梯度下降进行优化更新模型参数

```
#进行10次数据更新
For e in range(10):
Y_ = linear_model(x_train)
Loss = get_loss(y_,y_train)
w.grad.zero_()
#梯度归零
b.grad.zero_()
Loss.backward()
w.data = w.data -le-2*w.grad.data
b.data = b.data -le-2*bgrad.data
Print(‘epoch:{},loss:{}’,format(e,loss.data[0])
```

##3.项目实战
下面我们创建一个随机初始化的矩阵

```我们通过刚才的基本训练模型的流程来进行一次实战演练，
首先我们导入基本项目库
import torch 
import numpy as np 
from torch.autograd import Variable
#画图象
import matplotlib.pyplot as plt 
%matplotlib inline
```
1.导入相关数据
```
x_train = np.array([[3.3], [4.4], [5.5], [6.71], [6.93], [4.168], 
[9.779], [6.182], [7.59], [2.167], [7.042], 
[10.791], [5.313], [7.997], [3.1]], dtype=np.float32) 
y_train = np.array([[1.7], [2.76], [2.09], [3.19], [1.694], [1.573], 
[3.366], [2.596], [2.53], [1.221], [2.827], 
[3.465], [1.65], [2.904], [1.3]], dtype=np.float32)
转换Tensor
x_train = torch.from_numpy(x_train) 
y_train = torch.from_numpy(y_train)
```
2.定义W和b
```
w = Variable(torch.randn(1), requires_grad=True)
b = Variable(torch.zeros(1), requires_grad=True)
```
3.构建模型
```
x_train = Variable(x_train) 
y_train = Variable(y_train)
y_ = linear_model(x_train)
```
4.构建损失函数
```
def get_loss(y_, y): 
return torch.mean((y_ - y_train) ** 2)
loss = get_loss(y_, y_train)
```
5.反向传播求梯度
```
Loss.backward()
```
6.利用梯度下降优化
```
for e in range(10):
y_ = linear_model(x_train) 
loss = get_loss(y_, y_train) 
w.grad.zero_()
b.grad.zero_()
loss.backward() 
w.data = w.data - 1e-2 * w.grad.data
b.data = b.data - 1e-2 * b.grad.data
print('epoch: {}, loss: {}'.format(e, loss.data[0]))
```
7.查看结果
```
epoch:	0,	loss:	3.1357719898223877
epoch:	1,	loss:	0.3550889194011688
epoch:	2,	loss:	0.30295443534851074
epoch:	3,	loss:	0.30131956934928894
epoch:	4,	loss:	0.3006229102611542
epoch:	5,	loss:	0.29994693398475647
epoch:	6,	loss:	0.299274742603302
epoch:	7,	loss:	0.2986060082912445
epoch:	8,	loss:	0.2979407012462616
epoch:	9,	loss:	0.29727882146835327
```



**关于Datawhale**：

>Datawhale是一个专注于数据科学与AI领域的开源组织，汇集了众多领域院校和知名企业的优秀学习者，聚合了一群有开源精神和探索精神的团队成员。Datawhale以“for the learner，和学习者一起成长”为愿景，鼓励真实地展现自我、开放包容、互信互助、敢于试错和勇于担当。同时Datawhale 用开源的理念去探索开源内容、开源学习和开源方案，赋能人才培养，助力人才成长，建立起人与人，人与知识，人与企业和人与未来的联结。
