---
title: Python常用
tags:
  - 重要
  - python
isTop: true
abbrlink: 51276
---
---


## time_cal.py时间计算

使用：



```python
import time
from time_cal import *  # 自定义模块，计算代码运行时间



end_time = time.time()  # calculate time
time_calculation(start_time, time.time())
```



代码：

```python
import time
def time_calculation(start_time, end_time):
    end_time = time.time()  # calculate time
    if end_time - start_time >= 3600:
        print("{} h".format(round((end_time - start_time) / 3600, 2)))
    elif end_time - start_time >= 60:
        print("{} min".format(round((end_time - start_time) / 60, 2)))
    else:
        print("{} s".format(round((end_time - start_time), 1)))
```



## 河北工业大学p10 代码

利用SummaryWriter，Sequential，看流程图



```python
# coding:utf-8
import torch
from torch.nn import Sequential, Conv2d, MaxPool2d, Flatten, Linear
from torch.optim import optimizer
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F
import torch.optim as optim
import time
from time_cal import *  # 自定义模块，计算代码运行时间

# from main_09 import criterion
start_time = time.time()
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("device={}".format(device))

batch_size = 64
# 设置数据预处理方式（将图像数据转换为张量并进行归一化处理）
transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.1307,), (0.3081,))])  # 均值 标准差
# 加载训练数据集和测试数据集
train_dataset = datasets.MNIST(root='mnist', train=True, download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size)
test_dataset = datasets.MNIST(root='mnist', train=False, download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=False, batch_size=batch_size)


def Relu(x):
    pass


class Net(torch.nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = torch.nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = torch.nn.Conv2d(10, 20, kernel_size=5)
        a = torch.nn.ReLU
        self.pooling = torch.nn.MaxPool2d(2)
        self.fc = torch.nn.Linear(320, 10)
        self.model1 = torch.nn.Sequential(
            Conv2d(1, 10, kernel_size=5),
            MaxPool2d(2),
            torch.nn.ReLU(),
            Conv2d(10, 20, kernel_size=5),
            MaxPool2d(2),
            torch.nn.ReLU(),
            Flatten(),
            Linear(320, 10)
        )

    def forward(self, x):
        # Flatten data from (n, 1, 28, 28) to (n, 784)
        batch_size = x.size(0)  # 64
        # x = F.relu(self.pooling(self.conv1(x)))
        # x = F.relu(self.pooling(self.conv2(x)))
        x = self.model1(x)
        # x = x.view(batch_size, -1)  # flatten
        # x = self.fc(x)
        return x


model = Net()
model.to(device)
# 定义损失函数（交叉熵损失函数）
criterion = torch.nn.CrossEntropyLoss()
# 定义优化器（使用随机梯度下降法进行优化，设置学习率和冲量）
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
    print('Accuracy={}, [{}/{}],'.format(100 * correct / total, correct, total))


if __name__ == '__main__':
    for epoch in range(10):
        train(epoch)
        if epoch % 2 == 0:  # 求余函数
            test()
            # import time
            end_time = time.time()  # calculate time
            time_calculation(start_time, time.time())

print(model)
input = torch.ones((64, 1, 28, 28)).cuda()
output = model(input)
print(output.shape)
writer = SummaryWriter("../logs_seq")
writer.add_graph(model, input)
writer.close()
#  conda activate Deep39
# tensorboard --logdir=logs_seq

```

## 经典代码1

### p10.py

```python
# coding:utf-8
import torch
import shutil
from torch.nn import Sequential, Conv2d, MaxPool2d, Flatten, Linear, ReLU
from torch.optim import optimizer
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F
import torch.optim as optim
import time
from time_cal import *  # 自定义模块，计算代码运行时间

batch_size = 64
run_code = 1

start_time = time.time()
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("device={}".format(device))

# 设置数据预处理方式（将图像数据转换为张量并进行归一化处理）
transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.1307,), (0.3081,))])  # 均值 标准差
# 加载训练数据集和测试数据集
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
        self.model1 = torch.nn.Sequential(
            # Conv2d(1, 100, kernel_size=5), MaxPool2d(2), torch.nn.ReLU(),
            # Conv2d(100, 10, kernel_size=5), MaxPool2d(2), torch.nn.ReLU(),
            # Flatten(), Linear(160, 10),

            Conv2d(1, 100, kernel_size=3), MaxPool2d(2), ReLU(),
            Conv2d(100, 60, kernel_size=3), torch.nn.ReLU(),
            Conv2d(60, 40, kernel_size=4), MaxPool2d(2), torch.nn.ReLU(),
            Conv2d(40, 20, kernel_size=2), torch.nn.ReLU(),
            Flatten(), Linear(180, 10),

            # Conv2d(1, 10, kernel_size=5), MaxPool2d(2), torch.nn.ReLU(),
            # Conv2d(10, 20, kernel_size=5), MaxPool2d(2), torch.nn.ReLU(),
            # Flatten(), Linear(320, 10),
        )

    def forward(self, x):
        # Flatten data from (n, 1, 28, 28) to (n, 784)
        # batch_size = x.size(0)  # 64
        # x = F.relu(self.pooling(self.conv1(x)))
        # x = F.relu(self.pooling(self.conv2(x)))
        x = self.model1(x)
        # x = x.view(batch_size, -1)  # flatten
        # x = self.fc(x)
        return x


model = Net()
model.to(device)
# 定义损失函数（交叉熵损失函数）
criterion = torch.nn.CrossEntropyLoss()
# 定义优化器（使用随机梯度下降法进行优化，设置学习率和冲量）
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
    print('Accuracy={}, [{}/{}],'.format(100 * correct / total, correct, total))


if __name__ == '__main__':
    if run_code == 1:
        for epoch in range(10):
            train(epoch)
            if epoch % 2 == 0:  # 求余函数
                test()
                # import time
                end_time = time.time()  # calculate time
                time_calculation(start_time, time.time())

print(model)
input = torch.ones((64, 1, 28, 28)).cuda()
output = model(input)
print(output.shape)

# writer tensorboard
log_dir = "../logs_p10";  shutil.rmtree(log_dir, ignore_errors=True)
writer = SummaryWriter(log_dir); writer.add_graph(model, input);writer.close()
#  conda activate Deep39    tensorboard --logdir=logs_p10

```

### p11.py



```python
import shutil
import torch
from torch import nn
from torch.nn import Conv2d, MaxPool2d, ReLU, Flatten, Linear
from torch.optim import optimizer
from torch.utils.tensorboard import SummaryWriter
from torchvision import transforms
from torchvision import datasets
from torch.utils.data import DataLoader
import torch.nn.functional as F
import torch.optim as optim
import time
from time_cal import *  # 自定义模块，计算代码运行时间
start_time = time.time()
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
# device = torch.device("cpu")
print("device={}".format(device))

batch_size = 64
run_code = 0  # 1:run
transform = transforms.Compose([transforms.ToTensor(), transforms.Normalize((0.1307,), (0.3081,))])  # 均值 标准差

train_dataset = datasets.MNIST(root='mnist', train=True, download=True, transform=transform)
train_loader = DataLoader(train_dataset, shuffle=True, batch_size=batch_size)

test_dataset = datasets.MNIST(root='mnist', train=False, download=True, transform=transform)
test_loader = DataLoader(test_dataset, shuffle=False, batch_size=batch_size)


class InceptionA(nn.Module):
    def __init__(self, in_channels):
        super(InceptionA, self).__init__()
        # 1
        self.branch_pool = nn.Conv2d(in_channels, out_channels=24, kernel_size=1)
        # 2
        self.branch1x1 = nn.Conv2d(in_channels, out_channels=16, kernel_size=1)
        # 3
        self.branch5x5_1 = nn.Conv2d(in_channels, out_channels=16, kernel_size=1)
        self.branch5x5_2 = nn.Conv2d(16, out_channels=24, kernel_size=1)
        # 4
        self.branch3x3_1 = nn.Conv2d(in_channels, out_channels=16, kernel_size=1)
        self.branch3x3_2 = nn.Conv2d(16, 24, kernel_size=3, padding=1)
        self.branch3x3_3 = nn.Conv2d(24, 24, kernel_size=3, padding=1)
        self.inception_mode = torch.nn.Sequential(

        )

    def forward(self, x):
        # 1
        branch_pool = F.avg_pool2d(x, kernel_size=3, stride=1, padding=1)
        branch_pool = self.branch_pool(branch_pool)
        # 2
        branch1x1 = self.branch1x1(x)
        # 3
        branch5x5 = self.branch5x5_1(x)
        branch5x5 = self.branch5x5_2(branch5x5)
        # 4
        branch3x3 = self.branch3x3_1(x)
        branch3x3 = self.branch3x3_2(branch3x3)
        branch3x3 = self.branch3x3_3(branch3x3)
        # [batch, channel, width, height]
        outputs = [branch_pool, branch1x1, branch5x5, branch3x3] # 24 + 16 + 24 + 24 = 88 channel
        # outputs = [branch1x1, branch5x5, branch3x3, branch_pool]
        return torch.cat(outputs, dim=1)  # batch, channel(第一个), width, height. dim = -1是沿最后一个元素方向


class Net(nn.Module):
    def __init__(self):
        super(Net, self).__init__()
        self.conv1 = nn.Conv2d(1, 10, kernel_size=5)
        self.conv2 = nn.Conv2d(88, 20, kernel_size=5)
        self.incep1 = InceptionA(in_channels=10)
        self.incep2 = InceptionA(in_channels=20)
        self.mp = nn.MaxPool2d(2)
        self.fc = nn.Linear(1408, 10)
        self.model1 = torch.nn.Sequential(
            Conv2d(1, 10, kernel_size=5), MaxPool2d(2), ReLU(),
            InceptionA(in_channels=10),
            Conv2d(88, 20, kernel_size=5), MaxPool2d(2), ReLU(),
            InceptionA(in_channels=20),
            Flatten(), Linear(1408, 10),
        )

    def forward(self, x):
        # in_size = x.size(0)
        # x = F.relu(self.mp(self.conv1(x)))
        # x = self.incep1(x)
        # x = F.relu(self.mp(self.conv2(x)))
        # x = self.incep2(x)
        # x = x.view(in_size, -1)
        # x = self.fc(x)
        x = self.model1(x)
        return x


model = Net()

model.to(device)
print(model)
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
    print('Accuracy on test set: {}%, [{}/{}]'.format(100 * correct / total, correct, total))


if __name__ == '__main__':
    if run_code == 1:
        for epoch in range(10):
            train(epoch)
            if epoch % 2 == 0:
                test()
                import time
                time_calculation(start_time, time.time())


print(model)
input = torch.ones((64, 1, 28, 28)).cuda()
output = model(input)
print(output.shape)


log_dir = "../logs_11"; shutil.rmtree(log_dir, ignore_errors=True)
writer = SummaryWriter(log_dir); writer.add_graph(model, input); writer.close()
#  conda activate Deep39    tensorboard --logdir=logs_11
```

