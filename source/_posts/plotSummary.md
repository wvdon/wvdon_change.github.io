---
title: 'summary_about plot'
date: 2022-01-04
mathjax: true
Description: code template about plot picture 
---

tags: 
  - seabron
  - matplotlib
  - python

总结常用的画图：

导入包：

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```



`regplot和scatterplot`

```pyth
ax = plt.figure(figsize=(15, 10))
#data_plot = pd.DataFrame({"gene":data["gene"], "Cor":data["Cor"]}) 

#five line 
#Fig,Axes=plt.subplots(1)
col = data.columns
color = ['bisque','burlywood','tan','orange','wheat','gold']
pic_num = len(col)
for i in range(1,pic_num):
    a = sns.regplot(scatter=False,x = data[col[0]].astype(np.float64), y = data[col[i]].astype(np.float64), color=color[i],robust=True, ci=None,label = col[i])
    if i==1:
        b = sns.scatterplot(x=normal[col[0]].astype(np.float64),y = normal[col[i]].astype(np.float64), color='r',markers=['-'],label="non-im")
        c = sns.scatterplot(x=abnormal[col[0]].astype(np.float64),y = abnormal[col[i]].astype(np.float64), color='dodgerblue',markers=["x"],label='im')
    else:
        b = sns.scatterplot(x=normal[col[0]].astype(np.float64),y = normal[col[i]].astype(np.float64), color='r',markers=['-'])
        c = sns.scatterplot(x=abnormal[col[0]].astype(np.float64),y = abnormal[col[i]].astype(np.float64), color='dodgerblue',markers=["x"])
plt.ylabel("dna methylation",fontsize = 20)
plt.xlabel("rna expression",fontsize=20)
plt.title("ZNF703",fontsize=25)
plt.legend()
plt.show()
```

![14011641204207_.pic.jpg](https://s2.loli.net/2022/01/03/4AXkcb8C1j2UeLJ.png)