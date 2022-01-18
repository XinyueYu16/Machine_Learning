# 数据预处理——一致化/去量纲/

## 一致化
在评价模型中，使各特征的方向一致，统一为正指标（熵值法可自动处理），对于负指标转为正指标有以下两种方法：
- 倒数一致化：取倒数
- __减法一致化__：M - x, 其中M为人为规定的上界，不一定为0


## 去量纲
### 去量纲的原因
- 量纲过大的数值变量更容易在模型中起决定性影响（min-max）
- 量纲不一会降低迭代收敛速度（z-score）
- 依赖于距离的算法对于数据数量级非常敏感，eg Kmeans
### 去量纲的方法
- 归一化normalization(minmax): 
- 标准化z-score:
    - "等高线"会变圆
    - 标准化(Z-score standardization)数据迭代收敛更快
    - 在分类、聚类算法中，需要使用距离来度量相似性的时候、或者使用PCA技术进行降维的时候，标准化(Z-score standardization)表现更好
- 正则化regularization:
    - 使特征模为1，在计算特征间相似性时常用，主要应用于文本分类和聚类中。例如，对于两个TF-IDF向量的l2-norm进行点积，就可以得到这两个向量的余弦相似性

![归一化与标准化的对比](https://img-blog.csdn.net/20180718211936160?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQzODE0NjQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
![标准化的迭代收敛效果](https://img-blog.csdn.net/20180718215045347?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3UwMTQzODE0NjQ=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**_参考_**
- [标准化与归一化的收敛](https://www.cnblogs.com/ai-ldj/p/14257457.html)
- [正则化](https://blog.csdn.net/dengheng4891/article/details/101446368?spm=1001.2101.3001.6650.2&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.pc_relevant_default&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-2.pc_relevant_default&utm_relevant_index=5)