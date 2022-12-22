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

## ATAC-seq

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

This pipeline is designed for automated end-to-end quality control and processing of ATAC-seq and DNase-seq data.

#### **标准**

> 介绍前期质控指标，避免样本问题对后期实验结果的影响，造成错误或返工

**比对率：**

正常是超过95%，最低不能低于80%。

```shell
# 可以抽取部分样本在nt数据库中进行比对，看map到那些物种中，是否有部分细菌污染
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



```python
import os
import pandas as pd
import json
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

path= '/media/wvdon/data/wfy/atac/work/'
dirs = os.listdir(path)
aaa = []
for d in dirs:
    if not str(d).endswith("e"):
        json_path = f'/media/wvdon/data/wfy/atac/work/{d}/qc/qc.json'
        #print(d)
        data = ''
        with open(json_path, 'r') as f:
            data = json.load(f)
            #print(data)
        tss = data["align_enrich"]['tss_enrich']['rep1']['tss_enrich']
        map_read_prc =data["align"]['samstat']['rep1']['pct_mapped_reads']
        NRF = data["lib_complexity"]['lib_complexity']['rep1']['NRF']
        PBC1 = data["lib_complexity"]['lib_complexity']['rep1']['PBC1']
        aaa.append([d,tss,map_read_prc,NRF,PBC1])
an_data = pd.DataFrame(aaa,columns=['B',"tss","map_read_prc","NRF","PBC1"])

sns.kdeplot(an_data['PBC1'],cut=0,cumulative=True,shade=True,color="b")
#sns.kdeplot(an_data['map_read_prc'],cut=0,cumulative=True,shade=True,color="r")
plt.legend(title="PBC1")
plt.show()
an_data['PBC1'].hist()
```

![](https://web.wvdon.com/image/qc_python.png)



#### 差异Peak分析

差异peak是分析的第一步，也是基础。根据实验的设计，可以比较两个组之间差异的Peak.

以往的几篇文章都推荐使用[Diffbind](https://rdrr.io/bioc/DiffBind/man/DiffBind-package.html)(*Differential binding analysis of ChIP-seq peaksets*)

> 目前没有专门为ATAC设计的差异peak 分析工具，不过他们都是计算该区域的counts数据，归一化，对比两个组之间的差异。
>
> 另外HOMER, DBChIP，也能实现同样的需求。



#### Peak 注释

```python

import argparse
import math
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import sys
sys.path.append(r'/home/wvdon/BIO_ATAC/')
from common.pyShell import  runShell  
import os

def shellAccept():
    '''
    预定义命令行参数，接收并存储
    必须参数：None
    可选参数：
    -u / --URL
    -t / --threads
    -v / --version
    @return:返回获取到的命令行参数args，以数据字典格式
    '''
    try:    # 异常处理
        parser = argparse.ArgumentParser(description="peak 基因注释")
        #@todo 临时写成测试文件。
        parser.add_argument("-u", "--csv",required=False, type=str, help="peak csv path",default='/media/wvdon/sdata/test/12_19_peak_FDR005_remove4.csv')
        parser.add_argument("-v", "--version", type=str, help="工具版本号:V1.0")
        parser.add_argument("-output","--output", type=str,default='./annotate',help='输出路径')
        parser.add_argument("-annotatePeaks",default='/usr/local/share/bio/homer/bin/annotatePeaks.pl',help='-annotatePeaks.pl 执行路径')
        args = parser.parse_args()  # 获取参数字典
        return args
    except Exception as e:
        print(e)
def peakAnnotion(csv_path,work_path):
    bed_path=os.path.join(work_path,'outputPeak.bed')
    df = pd.read_csv(csv_path).iloc[:,1:11]
    
    df.columns = ['Chromosome', 'Start', 'End','width','Strand','C','N','P','Fold','pv']
    df[['Chromosome', 'Start', 'End','Strand']].to_csv(bed_path,header=None,sep='\t',index=0)
    
    shell = f'{args.annotatePeaks}  {bed_path} hg38 > {work_path}/outputPeakAnnotate.txt'
    print(f'exectue shell {shell}')
    status_code = runShell(shell,timeout = 120)
    if status_code==0:
        print(f'success annotate,export Peak Annotate file :{work_path}/outputPeakAnnotate.txt')
    else:
       print(f"Exectue '{shell}' Failed") 
    gene = pd.read_table(f'{work_path}/outputPeakAnnotate.txt',sep='\t')
    ids_list = []
    for k in gene.iloc[:,0]:
        if len(str(k))>1:
            ids_list.append(int(str(k)[2:])-1)
        else:
            ids_list.append(0)
    gene['ids'] = ids_list
    gene.sort_values('ids').to_csv(f'{work_path}/peak_outputPeakAnnotate_sorted.csv')
    
    atac_gene = gene.sort_values('ids')
    atac_gene['Fold']=df['Fold']

    output_gene_path = f'{work_path}/peak_outputPeakAnnotate_sorted_conact.csv'
    atac_gene.to_csv(output_gene_path)
    list_annotion = []
    for an in atac_gene['Annotation']:
        list_annotion.append(str(an).split(' (')[0])
    dict={}
    for key in list_annotion:
        dict[key]=dict.get(key,0)+1
    peakAnnotionPie(work_path,dict)
    peakUpDownPieAndBar(work_path,output_gene_path)
    return output_gene_path
def peakUpDownPieAndBar(work_path,data_path):
    data = pd.read_csv(data_path)
    fig = plt.figure()
    x = np.arange(0,math.pi*2,0.05)
    up=data[data['Fold']>0]['Fold']
    down = data[data['Fold']<0]['Fold']

    if len(up) > len(down):
        up_bins = 200
        down_bins = int(2000/len(up)*len(down))
        exp = (0,0.5)
        sits = 221
    else:
        down_bins = 200
        up_bins = int(2000/len(down)*len(up))
        ex = (0.5,0)
        sits = 222
    ax1 = fig.add_subplot(111)
    ax1.hist(down,bins=down_bins,color='orange')
    ax1.hist(up,bins=up_bins,color='red')
    ax2 = fig.add_subplot(sits,facecolor='r')
    ax2.pie([len(up),len(down)],shadow=True,colors=['orange','red'],explode=exp,labels=['colsed','open'],autopct='%1.1f%%')
    ax2.set_title(f'Total:{len(data)}')
    plt.savefig(f'{work_path}/percent_atac_pie.svg',dpi=300)
    print('plot annotion pie_bar done!')
    
def peakAnnotionPie(work_path,dict):
    expodes = (0,0,0.1,0,0,0,0,0.1)
    colors = ['red','orange','yellow','green','purple','blue','black','brown']
    plt.pie(dict.values(),explode=expodes,labels=dict.keys(),shadow=True,colors=colors,autopct='%1.1f%%')
    ## 用于显示为一个长宽相等的饼图
    plt.axis('equal')
    #保存并显示
    plt.savefig(f'{work_path}/pie_annotion.svg',dpi=300)
    print('plot annotion pie done!')


if __name__ == '__main__':
    root_path = os.getcwd()
    
    args = shellAccept()
    work_path = args.output
    csv_path = args.csv
    if not os.path.exists(work_path):
        os.makedirs(work_path)
    output_gene_path = peakAnnotion(csv_path,work_path)
```

```python
import subprocess
class pyShell():
    def __init__(self) -> None:
        pass
    '''
    return 0 : Success , else: Fail
    '''
def runShell(command,timeout=5):
    ret = subprocess.run(command,shell=True,stdout=subprocess.PIPE,stderr=subprocess.PIPE,encoding="utf-8",timeout=timeout)
    return ret.returncode 
```





**annotion nearby peaks**

```shell

```



#### Motif

peak注释虽然提供了功能解释，但并没有直接解释底层机制。开放的染色质可以通过转录因子影响转录，转录因子通过识别和结合 DNA 上的特定序列(*TFBS:TF 结合位点*)来促进转录。而事实上转录因子通过与组蛋白或非组蛋白的竞争以及与辅因子的合作来调节转录。

有两种类型的基序或基于 TF 的分析方法(**研究TF调控**)：

- 基序频率或活动的基于序列的预测
-  TF 占用的足迹。

JASPAR是现在用的最多的一个motif 数据库，事实上存的就是一些转录因子对应的位置权重矩阵（PWM），其中有的是实验的结果，还有的是计算预测出来的。

工具：

- TFBSTools
- **HOMER**
- MEME FIMO

原理都是一样的，基于PWM矩阵，然后在序列里面扫描搜索。

> 一直有一个疑问，对于motif扫描的时候，我们是应该用差异的区域，还是全部的区域？。差异的区域中全部的还是仅上调的区域？

```shell
/usr/local/share/bio/homer/bin/findMotifsGenome.pl out.bed hg38 first -len 6,8,10,12,14
```

> 对于hommer,其result 有两个，一部分是能够和已有数据库中，匹配到的，另外一部分是基于序列预测出来了的，可能没有任何的生物学意义。

#### TF Footprints

 除了motif，TF Footprints是另外一种研究转录因子调控的方法。原理是TF 与 DNA 结合会阻止结合位点内的 Tn5 切割，就会形成一个深渊低谷一样的峰值分布。

对于检测方法，目前都是基于Boyle 提出的变种隐马尔可夫模型HMM，即在每个碱基使用归一化和平滑的片段计数来检测不同的状态，例如足迹、侧翼和背景。其中目前用的比较多的是针对ATAC数据的HINT-ATAC。



最近的 HINT-ATAC 也使用 HMM，但只有 HINT-ATAC 校正了链特异性 Tn5 切割偏差

## RNA-seq 联合分析

通过 RNA-seq 定性或定量地将染色质可及性的变化与感兴趣的基因表达的变化联系起来，直观地，我们可以发现 DE 基因是否在相应的 TSS 周围也具有显着差异的染色质可及性，可以推断 DE 基因受与开放染色质中特定基序或足迹相关的 TF 调节.

![image-20221222165742726](https://web.wvdon.com/peca.png)

<center>Fig: Schematic overview of the method for constructing TF-chromatin transcriptional regulatory network/center>

转录因子TF，染色质调控因子CR和调控元件RE相互作用网络推断的新方法PECA，**可以使用 PECA<sup>[6]</sup> 方法重建调控网络。**中科院王勇教授团队，利用匹配的基因表达和染色质可及性数据刻画转录因子和调控元件结合调控下游基因表达的数学模型，构建了描绘细胞状态转化的染色质调控网络，通过网络分析鉴定出TFAP2C和p63分别为表面外胚层起始和角质形成细胞成熟的关键因子

## 总结



## 参考

1. Yan, Feng, et al. "From reads to insight: a hitchhiker’s guide to ATAC-seq data analysis." *Genome biology* 21.1 (2020): 1-16.
1. Krishnan, H. R. *et al.* Unraveling the epigenomic and transcriptomic interplay during alcohol-induced anxiolysis. *Mol. Psychiatry* (2022) doi:10.1038/s41380-022-01732-2.
1. Mok, G. F. *et al.* Characterising open chromatin in chick embryos identifies cis-regulatory elements important for paraxial mesoderm formation and axis extension. *Nat. Commun.* **12**, 1157 (2021).
1. RS ∗, GB †. DiffBind: Differential binding analysis of ChIP- Seq peak data. 
1. Li, Z., Schulz, M. H., Look, T., Begemann, M., Zenke, M., & Costa, I. G. (2019). [Identification of transcription factor binding sites using ATAC-seq](https://genomebiology.biomedcentral.com/articles/10.1186/s13059-019-1642-2). Genome Biology, 20(1), 45.
1. Duren Z, Chen X, Xin J, et al. Time course regulatory analysis based on paired expression and chromatin accessibility data[J]. Genome research, 2020, 30(4): 622-634.