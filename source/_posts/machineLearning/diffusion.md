---
title: 'Diffusion Models For Life Science'
date: 2023-2-8 10:11:41
tags:
  - bio
  - machineLearning
  - Generative_Learning
categories: machineLearning
mathjax: true
description: 扩散模型在生命科学上的应用与模型概括


---

## 引言

Diffusion models 在CV和NLP上大展风采。在蛋白设计上由于蛋白质主链几何结构和序列结构关系的复杂性限制了其应用。

## 背景

### **Protein Structure Key Task**

**Protein structure prediction**

- AlphaFold
- RosettaFold

**Protein design**

- ProteinMPNN
- RFjoint Inpainting
- RFDiffusion

![img](https://lh4.googleusercontent.com/v1Kch3n_sT-uC7_KbdbnIR87e4vnAb3nlJw1IPkm9QpRoerfu-LpPXeKj6ysfBW9cE10H-hdNjhVd9vQsjR6USTdN8HR6cULAhgnp3NMBHw2M_0OWydWYHHZ0OcJJUreXHL4HcBSXqgP_1A5IzK9YSq8SQ=s2048)

<center>In April 2019, Baker gave a TED talk titled "5 challenges we could solve by designing new proteins"</center>

### **Computational Protein Design Workflow**

![img](https://lh6.googleusercontent.com/nvabB164XGsMkiRp93XnDvaU8oHu7kQ3uYFV1zbFYbGI_U9549ZmVV0g9cMeCzfmUCAaeqPRXePdo7r0LDp8bHl8lOw4QLVGnthqaoSMtj6wDQeQBY4oAE4g2ph2aADjSXnUi-mA4nSMxmRuMKqLuRQEXw=s2048)



![img](https://lh3.googleusercontent.com/IM0S-rw3Mvefwyahtx6oJ8B-HfTUXUXmu-A-TVBY9LIXkK_L15YFDqvnSDbkwMT7WzP_uLoSDwZyT0U4hientvxPlAXqU7UlDGwa1zYyDjmB6SOSBXK0ou4dhCi9XI3HifbUFvTrT0JG3m3GrJmvz0rqpQ=s2048)

<center>Motifs can have various Functions and sources</center>



### **Evaluation for Designing proteins**

<img src="https://lh6.googleusercontent.com/FKpajpuYp7fHU1TJ_wkwMeoDSuYvyaoTamd7yexEOKCFgsNyh_SVtGdnnFuMMxCzBknlMBzddSwUV48UPSKTMZ6fPLMloV8flLMAGOTJ-hFiaPk-C7pkhASOidbZma1uaj-BFSrCVq0KOwc4Srvy3oyEwg=s2048" width = "150" height = "200" align=center/>



### **State-of-the-art**

<img src="https://web.wvdon.com/20230227223847.png"/>

<center>DALL-E2: An astronaut riding a horse in a photorealistic style</center>

![](https://web.wvdon.com/20230227224037.png)

<center>Imagen: A robot couple fine dining with Eiffel Tower in the background</center>



### **What makes this hard?**

**Post-AlphaFold, protein design is ‘guess’ & ‘check’**

- Naive guessing ? ~20^100 sequences
- !Native structures? Too sparseExisting 
- ML tools?
  - Low diversity
  - High compute cost
  - Short sequences is bad





## 模型详细介绍

### 生成模型

物理背景，搞物理的很牛，非平衡热力学。（熵增，混乱过程，逆转，从混乱中生成秩序。）

建模数据的生成概率。

GAN:生成器。判别器。对抗训练。

VAE:高维数据，近似。拟合

Flow:鲜艳分布  

Diffusion: 线性，隐变量

两个过程：

数据-》噪声，



![](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/generative-overview.png)

DDPM

**Forward diffusion process gradually adds noise to input data.**

**Reverse denoising process generates data by removing noise.**

缺点：



- 生成扩散模型的大火，则是始于2020年所提出的**[DDPM](https://link.zhihu.com/?target=https%3A//arxiv.org/abs/2006.11239)**（Denoising Diffusion Probabilistic Model）。
- DDPM的数学框架在2015年就已经完成了 ([Sohl-Dickstein et al., 2015](https://arxiv.org/abs/1503.03585))
- DDPM是首次将它在高分辨率图像生成上调试出来了，从而引导出了后面的火热(**DDPM**; [Ho et al. 2020](https://arxiv.org/abs/2006.11239)).

![](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/DDPM-algo.png)

The training and sampling algorithms in DDPM (Image source: [Ho et al. 2020](https://arxiv.org/abs/2006.11239))

### Forward diffusion process


$$
q(\mathbf{x}_t \vert \mathbf{x}_{t-1}) = \mathcal{N}(\mathbf{x}_t; \sqrt{1 - \beta_t} \mathbf{x}_{t-1}, \beta_t\mathbf{I}) \quad
q(\mathbf{x}_{1:T} \vert \mathbf{x}_0) = \prod^T_{t=1} q(\mathbf{x}_t \vert \mathbf{x}_{t-1})
$$


### Reverse diffusion process

反向过程就是通过估测噪声，多次迭代逐渐将被破坏的 x<sub>t</sub> 恢复成x<sub>0</sub>

### **如何训练**

### 如何使用

高斯贯穿全部；



KL散度。

## 应用

## 总结



词汇对应：

Denoising diffusion probabilistic models (DDPMs)：a powerful class of machine learning models recently demonstrated to generate novel photorealistic images in response to text prompts

## 参考

[What are Diffusion Models? | Lil'Log](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/) 

[Yang Song | Generative Modeling by Estimating Gradients of the Data Distribution](https://yang-song.github.io/blog/2021/score/) 

[Awesome-Diffusion-Models:This repository contains a collection of resources and papers on ***Diffusion Models***.](https://github.com/heejkoo/Awesome-Diffusion-Models#molecular-and-material-generation)