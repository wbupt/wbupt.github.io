---
title: 'Pytorch实践学习'
date: 2023-03-14 10:22:34
tags: [Python]
published: true
hideInList: true
feature: 
isTop: false
hide: false
---

I:\PyTorch深度学习实践 《PyTorch深度学习实践》完结合集

## P 1 Overview

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
Logistic regression Model 广播机制

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

```python
self.linear = torch.nn.Linear(8,2)
# 从8维空间 - 映射到 2维空间
8D - 24D - 12D 超参数搜索
```

`矩阵` 从n维空间映射到m维空间 

空间变换的函数

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

![image-20230422205932775](https://cdn.staticaly.com/gh/yangmulao/blogcdn@master/img/image-20230422205932775.png)



1. Input
2. Kernel
3. ###### Output

这个是普通的文本文字
**这个是粗体**
*这个是斜体*
`这个用于突出显示，或者说是高亮`
`a`

![image-20230314165217313](https://cdn.jsdelivr.net/gh/wbupt/blog1/image-20230314165217313.png)

卷积核有多少个

```python

; Typora
; 快捷增加字体颜色
; SendInput {Text} 解决中文输入法问题

#IfWinActive ahk_exe Typora.exe
{
    ; Ctrl+Alt+o 橙色
    ^!o::addFontColor("orange")

    ; Ctrl+Alt+r 红色
    ^!r::addFontColor("red")

    ; Ctrl+Alt+b 浅蓝色
    ^!b::addFontColor("cornflowerblue")
}

; 快捷增加字体颜色
addFontColor(color){
    clipboard := "" ; 清空剪切板
    Send {ctrl down}c{ctrl up} ; 复制
    SendInput {TEXT}<font color='%color%'>
    SendInput {ctrl down}v{ctrl up} ; 粘贴
    If(clipboard = ""){
        SendInput {TEXT}</font> ; Typora 在这不会自动补充
    }else{
        SendInput {TEXT}</ ; Typora中自动补全标签
    }
}
#IfWinActive ahk_exe Typora.exe
{
    ; Ctrl+Alt+K javaCode    
    ; crtl是  ^ , shift是 + , k是  k键
    ^+k::addCodeJava()
}
addCodeJava(){
Send,{```}
Send,{```}
Send,{```}
Send,python
Send,{Enter}
Return
}
```

`卷积核大小`
$$
n(个数)\times kernel\text{_size}_{width}(卷积核的大小)\times kernel\text{_size}_{height}(卷积核)
$$

```python
import torch
in_channels, out_channels= 5, 10
width, height = 100, 100
kernel_size = 3
batch_size = 1
input = torch.randn(batch_size, # ba
                    in_channels, # n
                    width,# W
                    height) # H
conv_layer = torch.nn.Conv2d(in_channels, # n
                             out_channels, # m
                             kernel_size=kernel_size) # (5,3)
output = conv_layer(input) # 卷积层

print(input.shape)
print(output.shape)
print(conv_layer.weight.shape)
```

![image-20230314171723480](https://cdn.jsdelivr.net/gh/wbupt/blog1/image-20230314171723480.png)

### `padding` 

补零 填充。一般(3*3)

```python
import torch
input = [3,4,6,5,7,
         2,4,6,8,2,
         1,6,7,8,4,
         9,7,4,6,2,
         3,7,5,4,1]
input = torch.Tensor(input).view(1, 1, 5, 5)
conv_layer = torch.nn.Conv2d(1, 1, kernel_size=3, padding=1, bias=False)
kernel = torch.Tensor([1,2,3,4,5,6,7,8,9]).view(1, 1, 3, 3) # 构造卷积核 # (O I W H)
conv_layer.weight.data = kernel.data
output = conv_layer(input)
print(output)
```

![padding=1](https://article.biliimg.com/bfs/article/5e9d93cce8a669b044868d5614e30ebb86293cac.png)

### `stride` 

卷积步长

### `MaxPooling` 

下采样：`最大`池化层:默认：stride=2

![](https://article.biliimg.com/bfs/article/98197f929812c18861300006c6e3f8692037061c.png)

可以理解为： `给图像打马赛克`

![](https://article.biliimg.com/bfs/article/457475798b3b37855b4e3e257a20929ec4f2b671.png)



![](https://article.biliimg.com/bfs/article/01145c6d488594cc0a0bedf8f4228a956921566f.png)

sdf

```python
import torch
in_channels, out_channels= 5, 10
width, height = 100, 100
kernel_size = 3
batch_size = 1
input = torch.randn(batch_size,
                    in_channels,
                    width,
                    height)
conv_layer = torch.nn.Conv2d(in_channels,
                             out_channels,
                             kernel_size=kernel_size)
output = conv_layer(input)

print(input.shape)
print(output.shape)
print(conv_layer.weight.shape)
```



df

```python
class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = torch.nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = torch.nn.Conv2d(10, 20, kernel_size=5)
        self.pooling = torch.nn.MaxPool2d(2)
        self.fc = torch.nn.Linear(320, 10)

    def forward(self, x):
        # Flatten data from (n, 1, 28, 28) to (n, 784)
        batch_size = x.size(0)
        x = F.relu(self.pooling(self.conv1(x)))
        x = F.relu(self.pooling(self.conv2(x)))
        x = x.view(batch_size, -1) # flatten
        x = self.fc(x)
        return x

model = Net()
device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")
model.to(device)
```

`Input Layer` .`Conv2d Layer `.`ReLU layer. ``Pooling Layer` .`Linear Layer `.`Output Layer `.

- CPU: time_cost: `1.74min`

<img src="https://article.biliimg.com/bfs/article/40801fd69a25f75aeaecab2712f2d0081c316958.png" style="zoom:80%;" />

- GPU: time_cost: `1.29min`

<img src="https://article.biliimg.com/bfs/article/f2cc0b433db07b6e33abf095e1fcce3a5b4a4c62.png" style="zoom: 80%;" />

`Compare CPU & GPU`

<img src="https://article.biliimg.com/bfs/article/c199f728e7be7a948a7a10d5f35e8fccc4a5f523.png" style="zoom: 80%;" />

```python
import torch
from torch.optim import optimizer
from torchvision import transforms
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F
import torch.optim as optim
import time

# from main_09 import criterion
start_time = time.time()
# Important
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")  # Important
print("device={}".format(device))  # Important

batch_size = 64
transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.1307,), (0.3081,))])  # 均值 标准差
train_dataset = datasets.MNIST(root='mnist', train=True, download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size)
test_dataset = datasets.MNIST(root='mnist', train=False, download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=False, batch_size=batch_size)


class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = torch.nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = torch.nn.Conv2d(10, 20, kernel_size=5)
        self.pooling = torch.nn.MaxPool2d(2)
        self.fc = torch.nn.Linear(320, 10)

    def forward(self, x):
        # Flatten data from (n, 1, 28, 28) to (n, 784)
        batch_size = x.size(0)
        x = F.relu(self.pooling(self.conv1(x)))
        x = F.relu(self.pooling(self.conv2(x)))
        x = x.view(batch_size, -1)  # flatten
        x = self.fc(x)
        return x


model = Net()
model.to(device)

# Construct criterion and optimizer
criterion = torch.nn.CrossEntropyLoss()
optimizer = optim.SGD(model.parameters(), lr=0.01, momentum=0.5)  # 冲量0.5


def train(epoch):
    running_loss = 0.0
    for batch_idx, data in enumerate(train_loader, 0):
        inputs, target = data
        inputs, target = inputs.to(device), target.to(device)
        optimizer.zero_grad()
        # forward + backward + update
        outputs = model(inputs)
        loss = criterion(outputs, target)
        loss.backward()
        optimizer.step()
        running_loss += loss.item()
        if batch_idx % 300 == 299:
            print('[%d, %5d] loss: %.3f' % (epoch + 1, batch_idx + 1, running_loss / 2000))
            running_loss = 0.0


def test():
    correct = 0
    total = 0
    with torch.no_grad():
        for data in test_loader:
            inputs, target = data
            inputs, target = inputs.to(device), target.to(device)
            outputs = model(inputs)
            _, predicted = torch.max(outputs.data, dim=1)
            total += target.size(0)
            correct += (predicted == target).sum().item()
    print('Accuracy on test set: %d %% [%d/%d]' % (100 * correct / total, correct, total))


if __name__ == '__main__':
    for epoch in range(10):
        train(epoch)
        if epoch % 2 == 0:
            test()
            import time

            end_time = time.time()  # calculate time
            if end_time - start_time >= 3600:
                print("{} h".format(round((end_time - start_time) / 3600, 2)))
            elif end_time - start_time >= 60:
                print("{} min".format(round((end_time - start_time) / 60, 2)))
            else:
                print("{} s".format(round((end_time - start_time), 1)))

```

## P11 卷积神经网络(高级篇)

### Google network

![](https://article.biliimg.com/bfs/article/4bf292fe9482243132b60b6ec611ecc7594b0e4d.png)

`不知道哪个好用，每个都用一下，全都要`

保持图像的宽度（W）和高度（H）保持一致。



![](https://article.biliimg.com/bfs/article/9bda660a8fb848119de0a0d4b9a453681404be1a.png)

`Averaage Pooling`:　均值池化。padding, stride 

![](https://article.biliimg.com/bfs/article/34020b1a67736ebb0803f9d57362e2e325097886.png)

### 1 * 1的卷积的作用

信息融合

- 取决于输入张量的通道

![](https://article.biliimg.com/bfs/article/4c3ceadd3b27afa70034c602e6b6d1c4cda62805.png)

`卷积后的中间的值，融合了三个图像中三个图像中间值。`

|       | 语文 | 数学 | 物理 | 化学 | 生物 |            |
| ----- | ---- | ---- | ---- | ---- | ---- | ---------- |
| 权重  | 1    | 0.8  | 0.9  | 1    | 0.7  | 加权       |
| 同学a | 120  | 135  | 96   | 87   | 88   | 加权后总分 |
| 同学b | 110  | 128  | 80   | 90   | 100  | 加权后总和 |

![](https://article.biliimg.com/bfs/article/c28576a572cab61cbfa9b39942c98b381ebe8a04.png)

1. A计算量：$5^2*28^2*192*32=1.2042.2400$
2. B计算量：$1^2*28^2*196*16+5^2*28^2*16*32=1243.3648$

![](https://article.biliimg.com/bfs/article/c75a0e3af7884d288f5dd2d22175cd2e42febcb3.png)

```python
self.branch_pool = nn.Conv2d(in_channels, 24, kernel_size=1) # 池化分支

branch_pool = F.avg_pool2d(x, kernel_size=3, stride=1, padding=1)
branch_pool = self.branch_pool(branch_pool)
```

```python
self.branch1x1 = nn.Conv2d(in_channels, 16, kernel_size=1)

branch1x1 = self.branch1x1(x)
```

```python
self.branch5x5_1 = nn.Conv2d(in_channels,16, kernel_size=1)
self.branch5x5_2 = nn.Conv2d(16, 24, kernel_size=5, padding=2)

branch5x5 = self.branch5x5_1(x)
branch5x5 = self.branch5x5_2(branch5x5)
```

```python
self.branch3x3_1 = nn.Conv2d(in_channels, 16, kernel_size=1)
self.branch3x3_2 = nn.Conv2d(16, 24, kernel_size=3, padding=1)
self.branch3x3_3 = nn.Conv2d(24, 24, kernel_size=3, padding=1)

branch3x3 = self.branch3x3_1(x)
branch3x3 = self.branch3x3_2(branch3x3)
branch3x3 = self.branch3x3_3(branch3x3)
```

![](https://article.biliimg.com/bfs/article/d8386091e74f74280cfd0fef5dfc4d204bd6065e.png)

```python
outputs = [branch1x1, branch5x5, branch3x3, branch_pool]
return torch.cat(outputs, dim=1)
```

```python
class InceptionA(nn.Module):
    def __init__(self, in_channels):
        super(InceptionA, self).__init__()
        self.branch1x1 = nn.Conv2d(in_channels, 16, kernel_size=1)
        
        self.branch5x5_1 = nn.Conv2d(in_channels,16, kernel_size=1)
        self.branch5x5_2 = nn.Conv2d(16, 24, kernel_size=5, padding=2)
        
        self.branch3x3_1 = nn.Conv2d(in_channels, 16, kernel_size=1)
        self.branch3x3_2 = nn.Conv2d(16, 24, kernel_size=3, padding=1)
        self.branch3x3_3 = nn.Conv2d(24, 24, kernel_size=3, padding=1)
        
        self.branch_pool = nn.Conv2d(in_channels, 24, kernel_size=1)
    def forward(self, x):
        branch1x1 = self.branch1x1(x)
        
        branch5x5 = self.branch5x5_1(x)
        branch5x5 = self.branch5x5_2(branch5x5)
        
        branch3x3 = self.branch3x3_1(x)
        branch3x3 = self.branch3x3_2(branch3x3)
        branch3x3 = self.branch3x3_3(branch3x3)
        
        branch_pool = F.avg_pool2d(x, kernel_size=3, stride=1, padding=1)
        branch_pool = self.branch_pool(branch_pool)
        
        outputs = [branch1x1, branch5x5, branch3x3, branch_pool]
        return torch.cat(outputs, dim=1)
```

```python
class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = nn.Conv2d(88, 20, kernel_size=5)
        self.incep1 = InceptionA(in_channels=10)
        self.incep2 = InceptionA(in_channels=20)
        self.mp = nn.MaxPool2d(2)
        self.fc = nn.Linear(1408, 10)
        
    def forward(self, x):
        in_size = x.size(0)
        x = F.relu(self.mp(self.conv1(x)))
        x = self.incep1(x)
        x = F.relu(self.mp(self.conv2(x)))
        x = self.incep2(x)
        x = x.view(in_size, -1)
        x = self.fc(x)
        return x
```

### 更上一层楼

1. 理论角度 《程度学习 花书》
2. 阅读PyTorch文档（通读文档）
3. 复现经典工作（读代码，写代码）
4. 扩充视野