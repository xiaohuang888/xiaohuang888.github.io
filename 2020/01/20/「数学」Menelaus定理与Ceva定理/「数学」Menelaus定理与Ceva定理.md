---
title: 「数学」Menelaus定理与Ceva定理
categories: 数学
tags:
    - 数学
    - 三角形
mathjax: true
---

### 内容

#### $\rm Menelaus$定理

![](https://s2.ax1x.com/2020/01/21/1F6YfH.png)

已知三角形$\triangle ABC$被一直线所截，交三条边或三条边的延长线与点$X, Y, Z$点，则有

$$\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$$

（注：上图为一种情况，还有一种为“直线不经过三角形的任何一边，即与三角形的交点数为$0$”）

**证明：**

![](https://s2.ax1x.com/2020/01/23/1Ezerd.png)

过点$C$作$CP // DF$交$AB$于$P$，则

$$\frac{BZ}{ZC}=\frac{BX}{XP}\tag{1}$$

$$\frac{CY}{YA}=\frac{PX}{XA}\tag{2}$$

$$(1) \times (2) \rm{得：} \frac{BZ}{ZC}\cdot \frac{CY}{YA}=\frac{BX}{XP}\cdot \frac{PX}{XA}$$

$$\frac{AX}{XB}\cdot\frac{BZ}{ZC}\cdot\frac{CY}{YA}=1$$

#### $\rm Menelaus$逆定理

若有三点$X$、$Y$、$Z$分别在边三角形的三边$AB$、$BC$、$CA$或边的延长线上，并且满足$\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$，那么$X$、$Y$、$Z$三点共线。

**证明：**

![](https://s2.ax1x.com/2020/01/23/1Vp0vF.png)

假设$X$、$Y$、$Z$三点不共线，直线$ZY$与$AB$交于点$P$。

根据$\rm Menelaus$定理，

$$\frac{AP}{PB}\cdot\frac{BZ}{ZC}\cdot\frac{CY}{YA}=1$$

$$\rm{已知}\frac{AX}{XB}\cdot\frac{BZ}{ZC}\cdot\frac{CY}{YA}=1$$

$$\therefore \frac{AP}{PB}=\frac{AX}{XB}$$

$$\therefore P \rm{与} X \rm{重合，即}X\rm{、}Y\rm{、}Z\rm{三点共线}$$

#### $\rm Ceva$定理

![](https://s2.ax1x.com/2020/01/22/1ESDV1.png)

在三角形$\triangle ABC$任取一点$O$，延长$AO$、$BO$、$CO$分别交对边于$x$、$y$、$z$，则有

$$\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$$

**证明：**

$\therefore \triangle ADC$被直线$BE$所截，

根据$\rm Menelaus$定理，

$$\therefore \frac{CB}{BZ}\cdot\frac{ZO}{OA}\cdot\frac{AY}{YC}=1\tag{1}$$

$\therefore \triangle ABD$被直线$CX$所截，

$$\therefore \frac{BC}{CZ}\cdot\frac{ZO}{OA}\cdot\frac{AX}{XB}=1\tag{2}$$

$$\frac{(2)}{(1)} \rm{得：}\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$$

#### $\rm Ceva$逆定理

若有三点$X$、$Y$、$Z$分别在边三角形的三边$AB$、$BC$、$CA$或边的延长线上，并且满足$\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$，那么$CX$、$BY$、$AZ$三线共点。

**证明：**

![](https://s2.ax1x.com/2020/01/23/1VCXng.png)

延长$CO$交$AB$于点$P$，则有

$$\frac{AP}{PB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$$

$$\rm{已知}\frac{AX}{XB} \cdot \frac{BZ}{ZC} \cdot \frac{CY}{YA}=1$$

$$\therefore \frac{AP}{PB}=\frac{AX}{XB}$$

$$\therefore P \rm{与} X \rm{重合，即}CX\rm{、}BY\rm{、}AZ\rm{三线共点}$$