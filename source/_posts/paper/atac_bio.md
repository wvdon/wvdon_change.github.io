---
title: '生信数据挖掘：ATAC-seq,RNA-seq'
date: 2022-12-19 00:11:41
tags:
  - bio
  - atac
categories: bio
mathjax: true
description: 描述ATAC-seq与RNA-seq数据挖掘与联合分析的思路和心得总结。能够反复回顾，检查自己做的是否有些许错误的地方，另外也能过给想要了解的同学提供一些思路，避免走太多弯路。

---

# ATAC-seq

### 背景

**Chromatin Accessibility**

人的DNA链全部展开大约有2m，需要折叠为染色质结构才可以存储到放到细胞核中。染色质的基本结构单位是核小体，核小体再折叠能形成高度压缩的染色质结构。这个过程像我们将文件压缩为zip或者rar的压缩包，只要在使用的时候才会解压出来，平时可以减少它的占用空间。



![](/Users/wuweidong/code/wvdon.github.io/source/_posts/paper/assets/chrom.png)

<center>Fig 1: Chromatin Accessibility</center>

高度折叠的染色质结构在复制和转录时需要暴露出DNA序列，这段暴露的区域就是染色质开放区域，**这个区域可以供转录因子和其他调控元件结合**，所以它与转录调控是密切相关的。

因此这种致密的核小体结构被破坏后，启动子、增强子等顺式调控元件和反式作用因子可以接近的特性，叫染色质开放性（**Chromatin Accessibility**）。

为了研究**染色质的开放性**，目前有MNase-seq,Dnase-seq,ATAC-seq等，但是目前最常用的是2013年由斯坦福大学开发的ATAC-seq。与传统的MNase-seq以及DNase-seq相比，其具有可重复性强，实验步骤简单，需要的实验样本量少等优点，因而被广泛应用<sup>1</sup>。

![image-20221219190711398](/Users/wuweidong/code/wvdon.github.io/source/_posts/paper/assets/image-20221219190711398-1508244.png)

<center> Fig 2: Methods of Researching Chromatin Accessibility a nd ATAC-seq principle</center>

**原理：**

利用转座酶Tn5会携带特定的已知序列，并且可以结合开放的染色质。Tn5酶对染色质开放区进行打断，在打断的同时加上测序接头，接着进行DNA提取，PCR扩增构建文库。经过测序分析，就可以推断染色质可行性、转录因子结合位点、组蛋白修饰区域和核小体位置。

![image-20221219193308860](/Users/wuweidong/code/wvdon.github.io/source/_posts/paper/assets/image-20221219193308860-1508259.png)

<center>Fig 3: The Process of Tn5</center>

### 分析

目前已经开源了很多ATAC-seq原始data的预处理与计算，其基本流程为：

**QC->Alignment->Remove low quality-> Call Peak** 

针对Call Peak 的结果，可以计算不同组间差异的Peak，或者Motif 富集与转录因子足迹分析，跟进一步的可以联合RNA-seq,

![image-20221219191431029](/Users/wuweidong/code/wvdon.github.io/source/_posts/paper/assets/image-20221219191431029-1508275.png)



Pipeline:

1. [ENCODE ATAC-seq pipeline](https://github.com/ENCODE-DCC/atac-seq-pipeline)

2. [ATAC-seq Guidelines form Harvard](https://informatics.fas.harvard.edu/atac-seq-guidelines.html)

   

# RNA-seq







# 联合分析





# 总结





# 参考

1. Yan, Feng, et al. "From reads to insight: a hitchhiker’s guide to ATAC-seq data analysis." *Genome biology* 21.1 (2020): 1-16.