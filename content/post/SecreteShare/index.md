---
title: 秘密分享
subtitle: 秘密分享数学定义介绍

# Summary for listings and search engines
summary: 记录DH秘密分享方法的了解.

# Link this post with a project
projects: []

# Date published
date: '2022-11-05T00:00:00Z'

# Date updated
lastmod: '2022-06-27T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption: 'Image credit: [**Unsplash**](https://unsplash.com/photos/CpkOjOcXdUY)'
  focal_point: ''
  placement: 2
  preview_only: false

authors:
  - admin
#  - 吳恩達

#links:
#  - icon: PDF
#    icon_pack: fab
#    name: PDF
#    url: 
tags:
  - 基础知识
  - MPC

categories:
   - MPC
#  - Demo
#  - 教程

---

# 

# 秘密共享

Secret Sharing

有一个秘密，及整数$D$，不希望泄漏。目前有$N$个人，将$D$进行随机切分并分给$N$个人，需要用到信息时就将$N$个部分信息聚合到一起，解密出$D$的值。

> 这种方案比较脆弱，如果一方不再，那么该秘密无法解密。



为了健壮性，设计了一种新的模式，该模式下不需要所有人一起解密，只需要$K$个人，其中$1\le K\le N$。

> 该方法发表在19179年的《ACM Communication》上，作者是Shamir（RSA发明者之一，图灵奖获得者）
>
> 该方法称为：门限秘密共享方案（threshold secret sharing scheme）



所以秘密分享可以分为两类：

- 严格的秘密分享，所有人存在才可解密
- 阈值的秘密分享，满足阈值的人数即可解密



## 严格秘密共享

Additive Secret Sharing 加法秘密共享

计算逻辑：

- 用户A和用户B作为数据持有方，用户C需要知道用户A和用户B的和，但是A和B不想告知C各自持有的值。
- 选择$N$个分片Server（可以是第三方），用户A和用户B各自将持有的$x$和$y$随机切分成$N$份$\{[[x]]_1,...[[x]]_n\}$和$\{[[y]]_1,...,[[y]]_n \}$，并分配给第三方$S_1,...,S_n$.
- 用户C请求三方计算$N$个分片的和，$[[z]]_1=[[x]]_1+[[y]]_1,...,[[z]]_n=[[x]]_n+[[y]]_n$
- 用户C收集$N$个分片参与者的$[[z]]_1,...[[z]]_n$，并计算求和



Mulplication On Two Additive Shares 使用随机数添加的乘法运算

协议算法：摘自论文 STR: Secure Computation on Additive Shares Using the Share-Transform-Reveal Strategy

![image-20220816160636303](https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220816160636303.png)

计算逻辑

- 用户A和用户B作为数据的持有方，用户C需要知道用户A和用户B的数字乘积，但A和B并不想告知C
- 选择$N$个分片Server（可以是第三方），用户A和用户B各自将持有的$x$和$y$随机切分成$N$份$\{[[x]]_1,...[[x]]_n\}$和$\{[[y]]_1,...,[[y]]_n \}$，并分配给第三方$S_1,...,S_n$.
- 需要一个随机添加的两个数字$a$和$b$，以及$ab$的乘积$c$，可以由三方提供，用户ABC都不知晓
- 选择同样的$N$个分片，并且添加的数字$a、b、c$随机切分为$N$份，$\{[[a]]_1,...[[a]]_n\}$、$\{[[b]]_1,...,[[b]]_n \}$和$\{[[c]]_1,...,[[c]]_n \}$，分配给$S_1,S_2,...,S_n$

- 对公式$z=x\cdot y$，进行转换

  $z=x\cdot y=(x-a+a)(y-b+b)=(e+a)(f+b)=e\cdot f+e\cdot b+f\cdot a+a\cdot b$

  其中有：$e=x-a、f=y-b$

- 用户C通过参与者分片Server联合计算出$e、f$，这时的$e、f$属于公开数字。

  各个分片计算$[[e]]_i=[[x]]_i-[[a]]_i$，然后用户C进行求和，求解$e$，

  各个分片计算$[[f]]_i=[[y]]_i-[[b]]_i$，然后用户C进行求和，求解$f$

- 对于最终的求解

  $z=xy=e\cdot f+e\cdot b+f\cdot a + a\cdot b$

  这需要各个参与者分片Server计算公开值

  $e\cdot f$，两个公开的数字乘积，无需参与者分片Server的联合计算即可解密数据。

  $e\cdot b$，一个公开的数字乘一个秘密数字，需要每个分片乘以公开数字，即每个参与者计算$[[b]]_i\cdot e$然后用户C进行汇总求和，即可获取解密数据

  $f\cdot a$，同上，计算$[[a]]_i\cdot f$

  $a\cdot b$，即$c$ 的值，由于$c$的值通过秘密分享的方式给参与者分片，只需要通过假发密钥分享即可获取，各个分片的$[[c]]_i$给用户C汇总求和

  最后$C$将获得的上述部分进行累加，就获取了用户A与用户B各自持有数字的乘积，而没有泄露用户A与用户B的具体值。



复杂运算

从`Additive Secret Sharing `和`Mulplication On Two Additive Shares`实现了基于`SS`的加法与乘法的秘密分享，那么理论山基于加法和乘法可以计算出其他所有运算，包括指数、对数计算。

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220817095442461.png" alt="image-20220817095442461" style="zoom:50%;" />

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220817095454896.png" alt="image-20220817095454896" style="zoom:50%;" />

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220817102717985.png" alt="image-20220817102717985" style="zoom:50%;" />

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220817102728296.png" alt="image-20220817102728296" style="zoom:50%;" />



## 阈值秘密分享

阈值秘密分享的基本思路是，基于秘密分享的便携性与安全性的考量，如果有$n$个秘密分片，我们只需要$K(1\le K\le n)$，将它们手中的信息拼凑起来，就可以获取秘密。

做到这一点需要考虑的问题：

- 随机性：挑选的$k$个人，需要时随机挑选的$k$个人，不能是一个固定的选取
- 必然性：随机挑选的$k$个人，能够将需要的数字进行解密

> 什么技术可以实现？
>
> 线性代数中，$k$个参与者，相当于$k$个变量，那么需要$k$个方程就可以解出

计算逻辑

秘密数字：$D$

参与方：$n$个参与者，$D_1,D_2,...,D_n$

一个大的素数，$p$，约束这个素数要大于$D$和$n$

- 首先，构造出$n$个方程式，构造公式$D_i=q(i)=\sum_{j=0}^{j=k-1}a_i*i^j$,【来源自文章How to Share a Secret 】

  就是使用多项式操作

- 公式展开
  $$
  D_i=q(i)=D+a_1*i+...+a_{k-1}*i^{k-1}
  $$
  其中$a_0=D$

- 公式举例（假设$k$为3，$n$为10）

  $D_1=q(1)=D+a_1+a_2,其中a_0=D$

  $D_2=q(2)=D+2a_1+4a_2,其中a_0=D$

  ​                                   ...

  $D_{10}=q(10)=D+10a_1+100a_2,其中a_0=D$

- 然后，目前的方程组里面，还有$a_1\sim a_{k-1}$的$K-1$个变量，那么这几个变量如何获取呢

  根据Adi Shamir在ACM发布的《How to share a Secret》的描述：

  >  The coefficients $a_1,...,a_{k-1}$ in $q(x)$ are randomly chosen from a uniform distribution over the integers in $[0, p)$, 

  即在$[0,p)$均匀分布的范围中随机获取。

- 原始文章中对$D_1,D_2,..,D_n$进行了约束是大素数的取模，这个大素数$p$的作用更多是控制我们选择$a_1\sim a_{k-1}$的大小，防止数据过于膨胀(唯一解的问题，在构造的时候基本可以规避【可自行证明，注：系数行列式不等于零】)

- 最后，将这$n$个方程划给$n$个参与者。需要计算时，只需要将$k$个方程获取进行联合计算就可以计算出$a$，也就是秘密$D。$

例子：

**加密**

- 输入

  秘密数字：$D=88$

  参与方：$10个，D_1,D_2,...,D_10$

  大素数：$p=991$

  解密阈值：$k=3$

- 加密逻辑

  生成方程组：

  $D_1=q(1)=88+a_1+a_2$

  $D_2=q(2)=88+2a_1+4a_2$

  ...

  $D_{10}=q(10)=88+10a_a+100a_2$

- 基于素数生成$a_1\sim a_{10}$的值，如设定$a_1=1,a_2=2$，即得到$D_1,...,D_{10}$

  $D_1=91,D_2=98,...,D_{10}=298$

- 可见此时$D_i，i\le n$均小于素数说991，符合预设，将这10个方程分布发不到10个参与者分片Server

  【分片仅知道$D_i$的值和方程分布】

**解密**

- 随机三个参与者，拿出方程组成方程组进行解密，如1，2，10

  $D_1=q(1)=D+a_1+a_2=91$

  $D_2=q(2)=D+2a_1+4a_2=98$

  $D_{10}=q(10)=D+10a_a+100a_2=298$

- 对方程组求解，得到$D,a_1,a_2$的值。