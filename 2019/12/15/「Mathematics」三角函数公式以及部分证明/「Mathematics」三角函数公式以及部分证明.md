---
title: 「Mathematics」三角函数公式以及部分证明
categories: Mathematics
tags:
    - Mathematics
    - 三角函数
mathjax: true
---

### 定义

![](https://s2.ax1x.com/2019/12/15/QhClnK.png)

在$Rt\triangle ABC$中，如下有六个三角函数的定义：

#### 正弦：

$$\sin A = \frac{a}{c}$$

#### 余弦：

$$\cos A = \frac{b}{c}$$

#### 正切：

$$\tan A = \frac{a}{b}$$

#### 余切：

$$\cot A = \frac{b}{a}$$

#### 正割：

$$\sec A = \frac{c}{b}$$

#### 余割：

$$\csc A = \frac{c}{a}$$

### 关系 & 公式

#### 倒数关系

$$\cos \alpha \cdot \sec \alpha = 1$$

$$\sin \alpha \cdot \csc \alpha = 1$$

$$\tan \alpha \cdot \cot \alpha = 1$$

#### 平方关系

$$1 + \tan ^ 2 \alpha = \sec ^ 2 \alpha$$

$$1 + \cot ^ 2 \alpha = \csc ^ 2 \alpha$$

$$\sin^2 \alpha + cos ^ 2 \alpha = 1$$

#### 商的关系

$$\frac{\sin \alpha}{\cos \alpha} = \frac{\sec \alpha}{\csc \alpha} = \tan \alpha$$

$$\frac{\cos \alpha}{\sin \alpha} = \frac{\csc \alpha}{\sec \alpha} = \cot \alpha$$

#### 正弦定理

$$\frac{a}{\sin A} = \frac{b}{\sin B} = \frac{c}{\sin C} = 2R = D$$

$R$ 为三角形外切圆半径，$D$ 为三角形外切圆直径。

**证明：**

![](https://s2.ax1x.com/2019/12/25/lFqL0x.png)

如图在 $\triangle ABC$ 中可得 $\sin A = \frac{h}{b}$ 和 $\sin B = \frac{h}{a}$ 。

$$\therefore h = \sin A \times b, h = \sin B \times a \\\\ \therefore \sin A \times b = \sin B \times a \\\\ \therefore \frac{\sin A}{a} = \frac{\sin B}{b} \\\\ \therefore \frac{a}{\sin A} = \frac{b}{\sin B} \\\\ \textrm{同理：} \frac{a}{\sin A} = \frac{c}{\sin C} \\\\ \therefore \frac{a}{\sin A} = \frac{b}{\sin B} = \frac{c}{\sin C}$$

![](https://s2.ax1x.com/2019/12/29/lKnCNQ.png)

如图， $\triangle CDB$ 中线段 $CD$ 经过圆心，所以 $\angle CBD = 90 ^ \circ$ ， $CD = 2R$。

$$\therefore \sin A = \sin D = \frac{CB}{CD} = \frac{a}{2R} \\\\ \therefore \frac{a}{\sin A} = 2R \\\\ \textrm{同理：} \frac{b}{\sin B} = 2R, \frac{c}{\sin C} = 2R \\\\ \therefore \frac{a}{\sin A} = \frac{b}{\sin B} = \frac{c}{\sin C} = 2R = D$$

#### 余弦定理

#### 和角公式

$$\sin(\alpha + \beta) = \sin \alpha \cos \beta + \cos \alpha \sin \beta$$

$$\sin(\alpha - \beta) = \sin \alpha \cos \beta - \cos \alpha \sin \beta$$

$$\cos(\alpha + \beta) = \cos \alpha \cos \beta - \sin \alpha \sin \beta$$

$$\cos(\alpha - \beta) = \cos \alpha \cos \beta + \sin \alpha \sin \beta$$