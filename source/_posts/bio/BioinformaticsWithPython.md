---
title: 'Bioinformatics with Python'
date: 2023-3-01 10:11:41
tags:
  - bio
  - computational Biology
  - Python
categories: machineLearning
mathjax: true
description: Using Python to slove computational Biology Problems!总结用python解决计算生物学问题


---

## 引言







Prepare my software：

“Anaconda, as it has become the de-facto standard for data science and bioinformatics. Also, it is the distribution that will allow you to install software from **[Bioconda](https://bioconda.github.io/)**”

Package：

| Package   | Purpose        | Package                                          |                                  |
| --------- | -------------- | ------------------------------------------------ | -------------------------------- |
| pandas    |                | DendroPY                                         | phylogenetics                    |
| Numpy     |                | PyMol                                            | Molecular visualization          |
| Scipy     |                | scikit-learn                                     | ML tools                         |
| Biopython |                | Cpython                                          | High performance for Big data    |
| seaborn   |                | Numba                                            | High performance for Big data    |
| rpy2      | R interface    | Dask                                             | Parallel processing for Big Data |
| PyVCF     | NGS            | [jupytext](https://jupytext.readthedocs.io/)/lab |                                  |
| Pysam     | NGS            | R                                                |                                  |
| HTSeq     | NGS processing |                                                  |                                  |

<center>A table showing the various software packages that are useful in bioinformatics</center>



```shell
#01 install base conda env 
conda create -n bioinformatics_base python=3.10
conda activate bioinformatics_base
# why use base env? 不同类型的包太多了。新的任务可以clone base env ,在此基础上install special packages.
#we can use 
# conda create -n scikit-learn --clone bioinformatics_base
# conda activate scikit-learn & conda install scikit-learn

#02 add the bioconda and conda-forge channels to our source list
conda config --add channels bioconda
conda config --add channels conda-forge

#03 install packages
# install from requirements 
#conda list -e > reqiurements.txt
conda install --yes --file requirements.txt

#04 install R from conda
conda install rpy2 r-essentials r-gridextra
```

Requirements.txt

```
biopython==1.79
jupyterlab==3.2.1
jupytext==1.13
matplotlib==3.4.3
numpy==1.21.3
pandas==1.3.4
scipy==1.7.1
```



```shell
# env for R
create -n bioinformatics_r --clone bioinformatics_base
conda activate bioinformatics_r
conda install r-ggplot2=3.3.5 r-lazyeval r-gridextra rpy2

```







### Aligment

> “pysam, a Python wrapper to the SAMtools C API”
>

```shell
conda install –c bioconda pysam
```



pandas