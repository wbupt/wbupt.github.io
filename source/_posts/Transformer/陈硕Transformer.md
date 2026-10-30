---
title:  陈硕--Transformer-Attention Is All You Need
abbrlink: Tranformer_chen
tags: [Transformer]

---
# Transformer-Attention Is All You Need

为什么提出Transformer,它到底是何方神圣？

它到底是如何表现高的性能，是否值得去研究？

1. (1) Transformerself-attention
2. (2) 位置编码
3. (3) 网络编码模块
4. (4) 网络解码模块

![image-20240418120642326](https://s2.loli.net/2024/04/18/HXW1EfiBJrboDQd.png)

**为什么引入Transformer?**
Transformer模型使用了Self-Attention机制，不采用RNN顺序结构，使得模型可以并行化训练，
而且能够拥有全局信息。
说白了：**并行加速计算+学习全局信息**

![image-20240418121136374](https://s2.loli.net/2024/04/18/WnyJMDTkHBFsIRv.png)

CNN缺点：只能考虑有限的内容
CNN优点：并行化处理

![image-20240418121404965](https://s2.loli.net/2024/04/18/hwjCQJVX4BmAlNK.png)

如何既能并行又能学习到全局信息？→Self-Attention

![image-20240418121554091](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404181215168.png)

为什么提出Transformer,它到底是何方神圣？

它到底是如何表现高的性能，是否值得去研究？

1. (1)Transformerself-sttention
2. (2)位置编码
3. (3)网络编码模块
4. (4)网络解码模块

具体如何实现Self-Attention?(q、k、v计算)

## 系统框架

![image-20240418121659147](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404181216192.png)

---

具体如何实现Self-Attention?(q、k、v计算)

```python
q:q：query (to match others) 查询
qi Waai
k:k：key (to be matched) 匹配
ki =Wkai
v:v：information to be extracted 信息
vi=Wval
```

q：查询



![image-20240422222349223](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222223323.png)

如何获得qkv

![image-20240422222610310](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222226415.png)

```tex
d is the dim of g and k
```

Self-attention

![image-20240422222739442](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222227526.png)

Considering the whole sequence

数学推导并行计算机制！

![image-20240422222944188](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222229282.png)

![image-20240422223107651](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222231757.png)

(ignore vd for simplicity)

步骤①、②、③

![image-20240422223322500](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222233627.png)

QKV如何计算
![image-20240422223526570](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222235648.png)

![image-20240422223638154](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404222236256.png)

### 位置编码 

p24:06

![image-20240423105345813](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404231053905.png)

Positional Encoding !!!!!
位置编码嵌入与词嵌入相加？
$$W\cdot x_p^i=[W^I,W^P]\cdot\left[x^i\backslash\text{p}^i\right]=W^I\cdot x^i+W^P\cdot p^i=a^i+e^i$$
位置编码获取方式：可以通过训练得到，也可以使用公式得到；
$$\begin{matrix}\text{[Sin(x),cos(x)}&\text{Sin(2x),cos(2x)....]}\end{matrix}$$

$$PE\left(1\right)=\left[\sin\left(1/10000^{0/512}\right),\cos\left(1/10000^{0/512}\right),\sin\left(1/10000^{2/512}\right),\cos\left(1/10000^{2/512}\right),\ldots\right]$$

P27:35

为什么提出Transformer,它到底是何方神圣？
它到底是如何表现高的性能，是否值得去研究？

1. (1)Transformerself-sttention
2. (2)位置编码
3. (3)网络编码模块
4. (4)网络解码模块

![image-20240423105957216](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404231059315.png)

Add表示残差连接
(Residual Connection)用于防止网络退化
Norm表示Layer Normalization,用于对每一层的激活值进行归一化

![image-20240423110208180](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404231102283.png)

### Encoder 30



![image-20240423110228255](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404231102352.png)

接着是一个Feed Forward的前馈网络和一个Add\&Norm Layer.
所以，这一个绿色的block的前2个Layer操作的表达式为：
O1 Layer Normalization(I+Multi-head Self-Attention(I))
这一个绿色的block的后2个Layer操作的表达式为：
2=Layer Normalization(1+Feed Forward Network(O))
Block(I)=02
所以Transformer的Encoder的整体操作为：
Encoder(I)=Block(...Block(Block)（）)
quadN times

### Dencoder 31

![image-20240423110410631](https://cdn.jsdelivr.net/gh/beiyoudaxue/pic2023@main/img/202404231104724.png)

新加的attention多加了一个mask,因为训练时的output都是Ground Truth,这样可以确保预测第i个位置时不会接触到未来的信息。

![image-20240615191518467](https://s2.loli.net/2024/06/15/d2lNJsHW6MIb4Aq.png)