---
title: Pytorch实践学习
tags: [Python, 学习笔记]
published: true
hidden: true
isTop: false
abbrlink: 22120
date: 2023-03-14 10:22:34
feature:
top: 
---



## P 1 Overview

1. Knowledge bases涉及泵及涂布装置.提供一种耐久性优异,可排出高粘度液体的泵及涂装置.泵包括:壳体;挠性部件,与壳体同轴地配置在壳体内,收容液体;工作缸部,收容工作流体;柱塞,可相对于工作缸部往返移动地插入于工作缸部,向壳体与挠性部件的间隙供给工作流体而使液体从挠性部件排出;第一密封部件及第二密封部件,配置在$$a$$$工作缸部中的柱塞的插入部,与柱塞滑动接触.第一密封部件是配置在比第二密封部件更靠柱塞的顶端侧,且具有耐压功能的密封部件,第二密封部件是具有防漏功能的密封部件.

2. 3

3. 3423

涉及泵及涂布装置.提供一种耐久性优异,可排出高粘度液体的泵及涂布装置.泵包括:壳体;挠性部件,与壳体同轴地配置在壳体内,收容液体;工作缸部,收容工作流体;柱塞,可相对于工作缸部往返移动地插入于工作缸部,向壳体与挠性部件的间隙供给工作流体而使液体从挠性部件排出;第一密封部件及第二密封部件,配置在工作缸部中的柱塞的插入部,与柱塞滑动接触.第一密封部件是配置在比第二密封部件更靠柱塞的顶端侧,且具有耐压功能的密封部件,第二密封部件是具有防漏功能的密封部件. 

![image-20230306161838698](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230306161838698.png)

链式法则

Deep learning is not too difficult

basic algebra probability python

There are lots of deep learning framework

- starting from scratch do not be required
- enabled efficient and convient use of GPU
- lots of components of neural networks provided by framewokd

Popular deep learning frameworks

- theano
- Caffe
- Torch / Pytorch.

Dynamical graph

More flexible 

easy to debug

intuitive and cleaner code

More neural netoworkic

## 2 线性模型

1. DataSet
2. Mode
3. Training

training数据：平时练习题；

validating数据：诊断；

test数据：考试题

overfit 过拟合	泛化

What would be the best model for the data?

Linear model?

Training Loss (Error)

损失函数

mean sqrt error

###Mean Square Error（均方误差）

![image-20230306164815661](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230306164815661.png)

![image-20230306164833614](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230306164833614.png)

![image-20230306170357312](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230306170357312.png)

## 03 梯度下降算法

![image-20230306171118394](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230306171118394.png)


$$
cost(\omega)=\dfrac{1}{N}\sum_{n=1}^N(\hat y_n-y_n)^2
$$

- 注意：只能实现算法的局部最优


![image-20230307165249276](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230307165249276.png)

鞍点：梯度为0的点$a$

Do the update 
$$
\omega=\omega-\alpha\dfrac{\partial{cost}}{\partial\omega}
$$
公式更新$$a+b$$公式

Update weight by every grad of sample of train_set.$w=w-a{w}/{3}$接口

## 04 反向传播

Linear Model $\hat{y}=x*\omega$

Stochastic Gradient Descent $\omega=\omega-\alpha\dfrac{\partial loss}{\partial\omega}$

<img src="../../../AppData/Roaming/Typora/typora-user-images/image-20230307173428700.png" alt="image-20230307173428700" style="zoom:50%;" />

In PyTorch, Tensor is the important component in constructing dynamic computational graph.

It contains data and grad, which storage the value of node and gradient w.r.t loss respectively.

1<div class="class1 class2">

```
.box {
  word-wrap: break-word; /* 允许单词内换行 */
  overflow-wrap: break-word; /* 允许单词内换行 */
}
```

qnjugs

```
letter-spacing: 0px; /* 文字间水平距离 */
line-height: 25px; /* 文字间垂直距离 */
```



t1=clock;

t2=clock;tc(t2,t1);

## P6 逻辑斯蒂回归 Logistic Regression--分类问题

![image-20230310104957472](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310104957472.png)

**In classification, the output of model is the probability of input belongs to the exact clss.**

Logistic function: $\sigma(x)=\dfrac{1}{1+e^{-x}}$

在[-00, +00]区间的函数关系

![image-20230310105326421](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310105326421.png)

Logistic function

```matlab
ix = 1;
wave = -10:0.1:10
for x = wave(ix)
    y(ix) = 1 / (1 + exp(1)^(-x) );
    ix = ix + 1;
end

plot(wave,y,'b', 'LineWidth',.5);hold on;
colorbar;axis equal;hold on;colormap jet;
set(gca,'FontSize',30,'FontName','Times New Roman', 'LineWidth',1.5);
```

<img src="https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310110316830.png" alt="image-20230310110316830" style="zoom: 50%;" />

![image-20230310111136283](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310111136283.png)
$$
loss=-(y\log\hat y+(1-y)\log(1-\hat y))
$$
![image-20230310111303257](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310111303257.png)
$$
loss=-\dfrac{1}{N}\sum_{n=1}^N y_n\log\hat y_n+(1-y_n)\log(1-\hat y_n)
$$

```
criterion = torch.nn.BCELoss(size_average=False)
```

1. **Prepare Dataset** - we shall talk about this later
2. **Design model using Class** - inherit from nn.Module
3. **Construct loss and optimizer** - using PyTorch API 
4. **Training Cycle** - Forward, backward, update

```
import torch
import torch.nn.functional as F
import numpy as np
import matplotlib.pyplot as plt
# Prepare dataset
x_data = torch.Tensor([[1.0], [2.0], [3.0]])
y_data = torch.Tensor([[0], [0], [1]])
# Design model using Class
class LogisticRegressionModel(torch.nn.Module):
    def __init__(self):
        super(LogisticRegressionModel, self).__init__()
        self.linear = torch.nn.Linear(1, 1)

    def forward(self, x):
        y_pred = F.sigmoid(self.linear(x))
        return y_pred
model = LogisticRegressionModel()

# Construct loss and optimizer using PyTorch API
criterion = torch.nn.BCELoss(size_average=False)
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)
# Training cycle
for epoch in range(1000):
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print('epoch={}, loss={}'.format(epoch, loss.data))
    optimizer.zero_grad()
    loss.backward()
    optimizer.step()

x = np.linspace(0, 10, 200)
x_t = torch.Tensor(x).view((200, 1))
y_t = model(x_t)
y = y_t.data.numpy()
plt.plot(x, y)
plt.plot([0, 10], [0.5, 0.5], c='r')
plt.xlabel('Hours')
plt.ylabel('Probability of Pass')
plt.grid()
plt.show()
```

![image-20230310112103083](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310112103083.png)



## P 7  处理多维特征的输入

Logistic regression model$\hat{y}^{(i)}=\sigma(x^{(i)}*\omega+b)$

Logistic regression model$\hat{y}^{(i)}=\sigma(\sum\limits_{n=1}^{8}x_n^{(i)}\cdot\omega_n+b)$
$$
\sum\limits_{n=1}^8x_n^{(i)}\cdot\omega_n=\begin{bmatrix}x_1^{(i)}&...&x_8^{(i)}\end{bmatrix}\begin{bmatrix}\omega_1\\ \vdots\\ \omega_8\end{bmatrix}
$$
Logistic regression Model

![image-20230310112846700](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310112846700.png)

Sigmoid function is in an element-wise fashion.
$$
\begin{bmatrix}\hat{y}^{(1)}\\ \vdots\\ \hat{y}^{(N)}\end{bmatrix}=\begin{bmatrix}\sigma\bigl(z^{(1)}\bigr)\\ \vdots\\ \sigma\bigl(z^{(N)}\bigr)\end{bmatrix}=\sigma\bigl(\begin{bmatrix}z^{(1)}\\ \vdots\\ Z^{(N)}\end{bmatrix}\bigr)
$$

$$
z^{(1)}=\begin{bmatrix}x_1^{(1)}&\cdots&x_8^{(1)}\end{bmatrix}\begin{bmatrix}\omega_1\\ \vdots\\ \omega_8\end{bmatrix}+b
$$

$$
z^{(N)}=\begin{bmatrix}&\vdots\\ x_1^{(N)}&\cdots&x_8^{(N)}\end{bmatrix}\begin{bmatrix}\omega_1\\ \vdots\\ \omega_8\end{bmatrix}+b
$$

$$
\begin{bmatrix}Z^{(1)}\\ \vdots\\ Z^{(N)}\end{bmatrix}=\begin{bmatrix}x_1^{(1)}&...&x_8^{(1)}\\ \vdots&\ddots&\vdots\\ x_1^{(N)}&...&x_8^{(N)}\end{bmatrix}\begin{bmatrix}\omega_1\\ \vdots\\ \omega_8\end{bmatrix}+\begin{bmatrix}b\\ \vdots\\ b\end{bmatrix}
$$

矩阵化 向量化 便于运行

```
self.linear = torch.nn.Linear(8,1)
```

![image-20230310113341902](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310113341902.png)

<img src="https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310114142966.png" alt="image-20230310114142966" style="zoom: 80%;" />

1. 扣书本
2. 读文档，基本的架构（泛化能力）

Example: artificial neural network

![image-20230310114513426](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310114513426.png)

![image-20230310114608659](https://cdn.jsdelivr.net/gh/yangmulao/aya/image-20230310114608659.png)

Diabetes Prediction

1. Prepare dataset
2. Design model using Class
3. Construct loss and optimizer
4. Training cycle

```python
import numpy as np
import torch

xy = np.loadtxt('diabetes.csv.gz', delimiter=',', dtype=np.float32)
x_data = torch.from_numpy(xy[:, :-1])
y_data = torch.from_numpy(xy[:, [-1]])


class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()

    def forward(self, x):
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        x = self.sigmoid(self.linear3(x))
        return x


model = Model()

criterion = torch.nn.BCELoss(size_average=True)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

for epoch in range(100):
    # Forward
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print(epoch, loss.item())
    # Backward
    optimizer.zero_grad()
    loss.backward()
    # Update
    optimizer.step()
```

```python
import numpy as np
import torch

xy = np.loadtxt('diabetes.csv.gz', delimiter=',', dtype=np.float32)
x_data = torch.from_numpy(xy[:, :-1])
y_data = torch.from_numpy(xy[:, [-1]])


class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()

    def forward(self, x):
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        x = self.sigmoid(self.linear3(x))
        return x


model = Model()

criterion = torch.nn.BCELoss(size_average=True)
optimizer = torch.optim.SGD(model.parameters(), lr=0.1)

for epoch in range(100):
    # Forward前馈
    y_pred = model(x_data)
    loss = criterion(y_pred, y_data)
    print(epoch, loss.item())
    # Backward反馈
    optimizer.zero_grad()
    loss.backward()
    # Update更新
    optimizer.step()

```

可以使用各种各样的激活函数

## P 8 加载数据集

**Dataset and DataLoader**

```python
# Training clcle
for epoch in range(training_epochs):
    # Loop over all batches
    for i in range(totaol_batch):
```

- Definition: Epoch

One forward pass and one backward pass of all the training examples.

- Definition: Batch_Size

The number of training examples in one forward backward pass.

- Definition: Iteration

Number of passes, each pass using [**batch size**] number of examples.

```python
import torch
from torch.utils.data import Dataset
from torch.utils.data import DataLoader
class DiabetesDataset(Dataset):
    def __init__(self):
        pass
    # The expression, dataset[index], will call this magic function.
    def __getitem__(self, index):
        pass
    # This magic function returns length of dataset.
    def __len__(self):
        pass
# Construct DiabetesDataset object.
dataset = DiabetesDataset()
# Initialize loader with batch-size,shuffle, num_workers=2:读取两个进程process number.
train_loader = DataLoader(dataset=dataset,
                          batch_size=32,
                          shuffle=True,
                          num_workers=2)
```

Dataset is an abstract class. We can define our class inherited from this class.

几乎每个魔法方法是python的内置方法。方法都有对应的内置函数，或者运算符，对这个对象使用这些函数或者运算符时就会调用类中的对应魔法方法，可以理解为重写这些python的内置函

The implementation of multiprocessing is different on Windows, which uses **spawn** instead of **fork**.
So left code will cause: 

```python
train_loader = DataLoader(dataset=dataset,
                          batch_size=32,
                          shuffle=True,
                          num_workers=2)
……
for epoch in range(100):
    for i, data in enumerate(train_loader, 0):
        ……
```

```python
train_loader = DataLoader(dataset=dataset,
                          batch_size=32,
                          shuffle=True,
                          num_workers=2)
……
if __name__ == '__main__':
    for epoch in range(100):
        for i, data in enumerate(train_loader, 0):
            # 1. Prepare data
```

So we have to wrap the code with an if-clause to protect the code from executing multiple times.  

```python
class DiabetesDataset(Dataset):
    def __init__(self, filepath):
        xy = np.loadtxt(filepath, delimiter=',', dtype=np.float32)
        self.len = xy.shape[0]
        self.x_data = torch.from_numpy(xy[:, :-1])
        self.y_data = torch.from_numpy(xy[:, [-1]])
        
        def __getitem__(self, index):
            return self.x_data[index], self.y_data[index]
        def __len__(self):
            return self.len
        
dataset = DiabetesDataset('diabetes.csv.gz')
train_loader = DataLoader(dataset=dataset, batch_size=32, shuffle=True, num_workers=2)
```

```python
import numpy as np
import torch
from torch.utils.data import Dataset, DataLoader


# Prepare dataset
# Dataset and Dataloader
class DiabetesDataset(Dataset):  # Diabetes is inherited from abstract class Dataset. N行（8特征列，1目标列）
    def __init__(self, filepath):  # filepath路径
        xy = np.loadtxt(filepath, delimiter=',', dtype=np.float32)  # 读取32浮点数
        self.len = xy.shape[0]  # xy.shape = [N, 0]  xy.shape[0] = N
        self.x_data = torch.from_numpy(xy[:, :-1])
        self.y_data = torch.from_numpy(xy[:, [-1]])

    def __getitem__(self, index):  # The expression, dataset[index], will call this magic function.
        return self.x_data[index], self.y_data[index]

    def __len__(self):  # This magic function returns length of dataset.
        return self.len


dataset = DiabetesDataset('diabetes.csv.gz')
train_loader = DataLoader(dataset=dataset, batch_size=32, shuffle=True, num_workers=2)  # shuffle 打乱


# Design model using Class
# inherit from nn.Module
class Model(torch.nn.Module):
    def __init__(self):
        super(Model, self).__init__()
        self.linear1 = torch.nn.Linear(8, 6)
        self.linear2 = torch.nn.Linear(6, 4)
        self.linear3 = torch.nn.Linear(4, 1)
        self.sigmoid = torch.nn.Sigmoid()

    def forward(self, x):
        x = self.sigmoid(self.linear1(x))
        x = self.sigmoid(self.linear2(x))
        x = self.sigmoid(self.linear3(x))
        return x


model = Model()
# Construct loss and optimizer
# using PyTorch API
criterion = torch.nn.BCELoss(reduction='mean')
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

# Training cycle
# forward, backward, update
if __name__ == '__main__':
    for epoch in range(100):
        # for i, data in enumerate(train_loader, 0): # train_loader (x, y) 赋值在data里 8:25
        for i, (inputs, labels) in enumerate(train_loader, 0):  # train_loader (x, y) 赋值在data里 8:25
            # 1. Prepare data
            # inputs, labels = data
            # 2. Forward
            y_pred = model(inputs)
            loss = criterion(y_pred, labels)
            if i % 10 == 0:
                if epoch % 10 == 0:
                    print('epoch= {}, i= {}, loss= {}'.format(epoch, i, loss.item()))
            # 3. Backward
            optimizer.zero_grad()
            loss.backward()
            # 4. Update
            optimizer.step()
```

```python
import torch
from torch.utils.data import DataLoader
from torchvision import transforms
from torchvision import datasets


train_dataset = datasets.MNIST(root='../dataset/mnist',
                               train=True,
                               transform= transforms.ToTensor(),
                               download=True)
test_dataset = datasets.MNIST(root='../dataset/mnist',
                              train=False,
                              transform= transforms.ToTensor(),
                              download=True)
train_loader = DataLoader(dataset=train_dataset,
                          batch_size=32,
                          shuffle=True)
test_loader = DataLoader(dataset=test_dataset,
                         batch_size=32,
                         shuffle=False)
for batch_idx, (inputs, target) in enumerate(train_loader):
    ……
```

Build DataLoader for

Build a classifier using the DataLoader.

## P 9  多分类问题

How to design the neural network?  

There are 10 labels in minist dataset.

1. Linear layer
2. Sigmoid layer
3. Input layer

```python
import torch
import numpy as np
y = np.array([1, 0, 0])
z = np.array([0.2, 0.1, -0.1])
y_pred = np.exp(z) / np.exp(z).sum()
loss = (- y * np.log(y_pred)).sum()
print(loss)
y = torch.LongTensor([0])
z = torch.Tensor([[0.2, 0.1, -0.1]])
criterion = torch.nn.CrossEntropyLoss()
loss = criterion(z, y)
print(loss)
```

```python
import torch
criterion = torch.nn.CrossEntropyLoss()
Y = torch.LongTensor([2, 0, 1])
Y_pred1 = torch.Tensor([[0.1, 0.2, 0.9],
                        [1.1, 0.1, 0.2],
                        [0.2, 2.1, 0.1]])
Y_pred2 = torch.Tensor([[0.8, 0.2, 0.3],
                        [0.2, 0.3, 0.5],
                        [0.2, 0.2, 0.5]])
l1 = criterion(Y_pred1, Y)
l2 = criterion(Y_pred2, Y)
print("Batch Loss1 = ", l1.data, "\nBatch Loss2=", l2.data)
```

```python
x = x.view(-1, 784)
self.l1 = torch.nn.Linear(784, 512)
x
```

`48:00`部分

```python
def test():
    correct = 0
    total = 0
    with torch.no_grad():
        for data in test_loader:
            images, labels = data
            outputs = model(images)
            _, predicted = torch.max(outputs.data, dim=1)
            total += labels.size(0)
            correct += (predicted == labels).sum().item()
    print('Accuracy on test set: %d %%' % (100 * correct / total))
```

```python
if __name__ == '__main__':
    for epoch in range(10): # 一轮训练，一轮测试
        train(epoch)
        test()
```

`or`

```python
if __name__ == '__main__':
    for epoch in range(10): # 一轮训练，一轮测试
        train(epoch)
        if epoch % 10 = 9 # 每10轮测试一次
        	test()
```

## P 10 卷积神经网络(基础篇)

`a`

1. Convolution
2. Subsampling
3. Convolution
4. Subsampling
5. Fully Connected
6. Fully
7. Connected

`图像可以表示为像素`--RGB栅格图像 `P10-12:00`

``==a==``

![image-20230314114425455](I:\web\Gridea\md_picture\image-20230314114425455.png)

`标红`

==其他色==

<font color='orange'>橙色</font>

<font color='red'>红色</font>

<font color='cornflowerblue'>浅蓝色</font>

<font color='cornflowerblue'>a</font>

1. Input
2. Kernel
3. Output



