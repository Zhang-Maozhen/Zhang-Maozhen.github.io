---
title: 差分隐私
subtitle: 差分隐私介绍

# Summary for listings and search engines
summary: 差分隐私的概念、属性和历史整理

# Link this post with a project
projects: []

# Date published
date: '2022-07-5T00:00:00Z'

# Date updated
lastmod: '2022-07-5T00:00:00Z'

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: ture

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

tags:
  - 查分隐私
  - 学术

categories:
   - 查分隐私
   - 学术 
#  - Demo
#  - 教程
---

# 差分隐私

## 差分隐私概念

差分隐私是算法的属性，而不是数据的属性，我们证明一个算法满足差分隐私。

证明数据集满足差分隐私则必须证明产生它的算法满足差分隐私。

<font color="#FF0000">满足查分隐私的功能通常称为机制。如果对于所有相邻数据集$x,x'$，以及所有可能的输出$S$，则机制$F$满足差分隐私</font>
$$
\frac{Pr[F(x)=S]}{PR[F(x')=S]}{≤e^ε}
$$

$F$是一个随机函数，所以并不是一个点的分布，而是一个描述输出结果的概率分布

$F$内置的随即向宁应该是足够的，从$F$得到的输出不会揭示$x或x'$中的哪一个是输入；如果判断不出哪一个是输入，那么就无法判断数据是否存在于输入中

$ϵ$是隐私预算，较小的$\epsilon$要求$F$在给定相似输入时得到相似的输出，这样提供了更高级别的隐私，较大的${\epsilon}$允许的输出相似性较低，提供的隐私性较低 

- $ϵ$是隐私预算小，隐私性比较高
- $ϵ$是隐私预算大，隐私性比较低（隐私越少，是差分隐私差分出来的信息越少？  ）
- 一般而言，${\epsilon}$在1左右或更小，当${\epsilon}$大于10时对隐私保护没有多少作用

${\epsilon}$是差分隐私

### 拉普拉斯机制

差分隐私领域已经开发了一些基本机制，准确描述了使用什么样的噪声以及使用多少噪声，其中之一被称为拉普拉斯机制[^1]

------

拉普拉斯机制，对于返回数字的函数$f(x)$，$F(x)$的以下定义满足${\epsilon}$-差分隐私：
$$
F(x) = f(x) + Lap(\frac{s}{\epsilon})
$$
$s$是$f$的敏感度，$Lap(s)$表示从中心为0且尺度为$s$的拉普拉斯分布中采样。

------

函数f的敏感度是当输入变化1时f的输出变化量。【计数查询的灵敏度始终为1】

使用灵敏度为1和我们选择的$\epsilon$的拉普拉斯机制来实现差分隐私，选择$\epsilon=0.1$,使用`Numpy的random.laplace从拉普拉斯分布中采样`：

```python
sensitivity = 1
epsilon = 0.1

adult[adult['Age'] >= 40].shape[0] + np.random.laplace(loc=0, scale=sensitivity/epsilon)
```



## 差分隐私的属性

它具有三个重要属性：

- 顺序合成（Sequential composition）[^1][^2]
- 并行合成（Parallel composition）[^3]
- 后处理（Post processing）

### 顺序合成

$F_1(x)$满足$\epsilon_1$-差分隐私，$F_2(x)$满足$\epsilon_2$-差分隐私，那么这两个机制的结果$G(x)=(F_1(x),F_2(x))$满足$\epsilon_1+\epsilon_2$-差分隐私

顺序组合作为差分隐私的重要属性是因为它允许设计不止一次查询数据的算法。它可以使个人通过这些分析来约束产生的总隐私成本

顺序组合给出的隐私成本界限是一个上限，两个特定的不同的私有版本的实际隐私成本可能小于这个值，但不能大于这个值

顺序组合会产生多个版本的总${\epsilon}$上限，对隐私的实际积累影响可能更低，实际的因私损失似乎略低于顺序组合确定的上限${\epsilon}$。

### 并行合成

并行组合基于将数据集拆分为不相交的块，并分别在每个块上运行差分私有机制。块是不想交的，因此每个人的数据恰好出现在一个块中，即便总共有k个块（因此运行k次，一个块一次）该机制叶芝在每个人的数据上运行一次。

### 直方图统计

直方图对于差分隐私而言，自动满足并行组合。直方图中的每个'bin'都通过数据属性的可能值定义，即Education = HS-gard。单行不会同时有一个属性的两个值，因此这种方式定义bin可以保证它们是不想交的。因此满足了并行组合的要求，并且可以使用差分隐私机制释放所有bin计数，总隐私成本为${\epsilon}$

### 交叉表

一次计算数据集中具有多个属性的特定值的行的频率。经常用于分析数据时显示两个变量之间的关系。

## 局部敏感性

### 通过局部敏感性实现差分隐私

> Q：可以像全局敏感性一样使用拉普拉斯机制么？$F$的以下定义是否满足ε-差分隐私?
>
> A：$LS(f,x)$取决于数据集，如果分析师知道查询在数据集上的本地敏感性，那么他可以推断出数据集的一些信息

# 简记

Dwork2006年提出[^1]查分隐私

串行组合[^4]

# 文献

[^1]: Cynthia Dwork, Frank McSherry, Kobbi Nissim, and Adam Smith. Calibrating noise to sensitivity in private data analysis. In *Proceedings of the Third Conference on Theory of Cryptography*, TCC'06, 265–284. Berlin, Heidelberg, 2006. Springer-Verlag. URL: https://doi.org/10.1007/11681878_14, [doi:10.1007/11681878_14](https://doi.org/10.1007/11681878_14).
[^2]: Cynthia Dwork, Krishnaram Kenthapadi, Frank McSherry, Ilya Mironov, and Moni Naor. Our data, ourselves: privacy via distributed noise generation. In Serge Vaudenay, editor, *Advances in Cryptology - EUROCRYPT 2006*, 486–503. Berlin, Heidelberg, 2006. Springer Berlin Heidelberg.

[^3]: Frank D. McSherry. Privacy integrated queries: an extensible platform for privacy-preserving data analysis. In *Proceedings of the 2009 ACM SIGMOD International Conference on Management of Data*, SIGMOD '09, 19–30. New York, NY, USA, 2009. Association for Computing Machinery. URL: https://doi.org/10.1145/1559845.1559850, [doi:10.1145/1559845.1559850](https://doi.org/10.1145/1559845.1559850).

[^4]: McSherry, Frank. Privacy integrated queries[J]. Communications of the Acm, 2010, 53(9):89. .

