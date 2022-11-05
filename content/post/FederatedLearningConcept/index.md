---
title: 联邦学习 概念
subtitle: 联邦学习基础知识

# Summary for listings and search engines
summary: 联邦学习的发展起源及定义.

# Link this post with a project
projects: []

# Date published
date: '2022-07-29'

# Date updated
lastmod: '2022-07-29'

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

links:
  - icon: PDF
    icon_pack: fab
    name: PDF
    url: 
tags:
  - 联邦学习

categories:
   - 联邦学习
#  - Demo
#  - 教程
---



# 联邦学习  概念

传统的联邦学习的目标函数可写为：
$$
f(w)=\sum_{k=1}^K\frac{n_k}{n}F_k(w)\\
F_k(w)=\frac{1}{n_k}\sum_{i=1}^{n_k}l(h(x_i;w),y_i)
$$
$K$为总结点个数，$n_k$为第$k$个节点的样本数。

联邦学习训练过程中，会将数据按照Non-IID划分到各client节点，然后在各client节点的数据划分train/test/val。对于传统联邦学习而言，每个client都会使用全局模型$w$进行测试。

> 在IID条件下，在分布式优化中常假定$f(w)=\mathbb E_{D_k}[F_k(w)]$，其中$D_k$为第$k$个节点的数据集，此时就退化为传统的分布式机器学习。然而在数据Non-IID条件下，$F_k$就不是一个对$f$的良好近似，这意味着想训练一个模型$w$满足所有节点的要求难度很大

<font color="#ED526">【醒悟】：这就是联邦学习个性化么呀！</font>

> 联邦学习个性化不求构建全局通用的模型$w$，而是为每个节点分别创建一个个性化的模型$w_k$
>
> 个性化常见方法有：元学习、多任务学习、迁移学习





主要划分有：

> 横向联邦学习、纵向联邦学习、迁移联邦学习

最初是由谷歌的H.Brendan McMahan等人提出，并将其应用落地[^FedAvg]。

[^FedAvg]: Mcmahan H B, Moore E, Ramage D, et al. Communication-Efficient Learning of Deep Networks from Decentralized Data [J]. international conference on artificial intelligence and statistics, 2016.

**横向联邦学习**

适用于特征重合多，样本重合较少的数据集进行联合计算的场景。以样本维度（横向）对数据集进行切分，以特征相同而样本不完全相同的数据部分为对象进行训练。

> 谷歌在 2016 年提出的安卓手机模型更新数据联合建模方案就是利 用单个用户使用安卓手机时，不断在本地更新模型参 数并上传到安卓云上，从而使特征维度相同的各数据 拥有方联合建模

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/20220704225948.png" style="zoom:50%;" />

**纵向联邦学习**

适用于样本重合较多，而特征重合较少的数据集间联合计算的场景。以特征维度（纵向）对数据集进行切分，以样本相同而特征不完全相同的数据集部分为对象进行训练

> 如同一地区的银行和电商，两个机构在特定地区的用户群里交集较大，因此可以对不同维度的用户特征进行聚合以增强模型能力。

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220705095039461.png" alt="image-20220705095039461" style="zoom:50%;" />

**联邦迁移学习**

适用于数据集间样本和特征重合均较少的场景。在这样的场景中，不再对数据进行切分，而是利用迁移学习来弥补数据或标签的不足。

> 如不同地区、不同行业机构之间进行联合建模为例，用户群体和特征维度的交集都较小，联邦迁移学习即用来针对性解决单边数据规模小，标签样本少的问题。

<img src="https://mz-pico-1311932519.cos.ap-nanjing.myqcloud.com/image/image-20220705095641894.png" alt="image-20220705095641894" style="zoom:50%;" />