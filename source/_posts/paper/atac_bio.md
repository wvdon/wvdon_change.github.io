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



![](https://web.wvdon.com/image/chrom.png)

<center>Fig 1: Chromatin Accessibility</center>

高度折叠的染色质结构在复制和转录时需要暴露出DNA序列，这段暴露的区域就是染色质开放区域，**这个区域可以供转录因子和其他调控元件结合**，所以它与转录调控是密切相关的。

因此这种致密的核小体结构被破坏后，启动子、增强子等顺式调控元件和反式作用因子可以接近的特性，叫染色质开放性（**Chromatin Accessibility**）。

为了研究**染色质的开放性**，目前有MNase-seq,Dnase-seq,ATAC-seq等，但是目前最常用的是2013年由斯坦福大学开发的ATAC-seq。与传统的MNase-seq以及DNase-seq相比，其具有可重复性强，实验步骤简单，需要的实验样本量少等优点，因而被广泛应用<sup>1</sup>。

![image-20221219190711398](https://web.wvdon.com/image/image-20221219190711398.png)

<center> Fig 2: Methods of Researching Chromatin Accessibility a nd ATAC-seq principle</center>

**原理：**

利用转座酶Tn5会携带特定的已知序列，并且可以结合开放的染色质。Tn5酶对染色质开放区进行打断，在打断的同时加上测序接头，接着进行DNA提取，PCR扩增构建文库。经过测序分析，就可以推断染色质可行性、转录因子结合位点、组蛋白修饰区域和核小体位置。

![image-20221219193308860](https://web.wvdon.com/image/image-20221219193308860.png)

<center>Fig 3: The Process of Tn5</center>

### 分析

目前已经开源了很多ATAC-seq原始data的预处理与计算，其基本流程为：

**QC->Alignment->Remove low quality-> Call Peak** 

针对Call Peak 的结果，可以计算不同组间差异的Peak，或者Motif 富集与转录因子足迹分析，更进一步的可以联合RNA-seq。

![image-20221219191431029](https://web.wvdon.com/image/image-20221219191431029.png)



<center>Fig 4: Roadmap of a typical ATAC-seq analysis.</center>

Pipeline:

1. [ENCODE ATAC-seq pipeline](https://github.com/ENCODE-DCC/atac-seq-pipeline)

2. [ATAC-seq Guidelines form Harvard](https://informatics.fas.harvard.edu/atac-seq-guidelines.html)

#### **标准**

> 介绍前期质控指标，避免样本问题对后期实验结果的影响，造成错误或返工

**比对率：**

正常是超过95%，最低不能低于80%。

```shell
zcat ../data/B63_L4_Q803601.R1.fastq.gz | head -n 1000 >B63_1
zcat ../data/B63_L4_Q803601.R2.fastq.gz | head -n 1000 >B63_2
 
awk '{if(NR%4 == 1){print ">" substr($0, 2)}}{if(NR%4 == 2){print}}' B63_1 > B63_1.fasta
blastn -task blastn -query B63_1.fasta -db /home/nt -num_threads 6 -out unpaired_blastn.aln
# 本地构建db花费时间较多，可以线上。
#B63_atac：73.4
#B60_atac：77.3
#细菌污染
```



**峰值区域的读数比例（FRiP score）**：

FRiP score应大于0.3，最低不能低于0.2。

所有映射的读数中，属于被称为峰值区域的部分，即显著富集的峰值中的**可用读数除以所有可用读数**。一般来说，FRiP得分与区域的数量呈正相关.(Landt et al, Genome Research Sept. 2012, 22(9): 1813–1831)

**TSS 富集：**

TSS富集计算是一种信噪比计算。收集一组参考TSSs周围的读数，形成以TSSs为中心、向任一方向延伸2000bp（共计4000bp）的读数总分布。然后，该分布被归一化，即在分布的每个末端侧翼的100bps内取平均读数深度（总共200bp的平均数据），并计算每个位置相对于该平均读数深度的倍数变化。这意味着侧翼应该从1开始，如果在转录起始位点（基因组的高度开放区域）有高的读数信号，那么信号应该增加，直到中间的一个峰值。我们把这个归一化后的分布中心的信号值作为我们的TSS富集度量。用于评估ATAC-seq。

![](https://web.wvdon.com/image/tss_e.png)

<center>Fig 5: Transcription Start Site (TSS) Enrichment</center>

![](https://web.wvdon.com/image/tss.png)

<center>Fig 6: Transcription Start Site (TSS) Enrichment Standard Value</center>

**文库复杂度测量：**

理想状态值是: NRF>0.9, PBC1>0.9, and PBC2>3. 

![](https://web.wvdon.com/image/pbc.png)

<center>Fig 7: Non-Redundant Fraction etc. Standard Value</center>

**Non-Redundant Fraction (NRF)** – Number of distinct uniquely mapping reads (i.e. after removing duplicates) / Total number of reads.

**PCR Bottlenecking Coefficient 1 (PBC1)**

**PCR Bottlenecking Coefficient 2 (PBC2)**





# RNA-seq







# 联合分析





# 总结





# 参考

1. Yan, Feng, et al. "From reads to insight: a hitchhiker’s guide to ATAC-seq data analysis." *Genome biology* 21.1 (2020): 1-16.