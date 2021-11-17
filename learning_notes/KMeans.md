# KMeans

**标签**：非监督、聚类、快速、随机（local minima）简单好用（客户好理解）、sklearn

**应用**：

 - “机器分类”：如土地/客户RFM/波士顿矩阵

 - 特征处理：为数据增加分类特征——类似于增加信息权重？或者对数据分组建模

   ​	*是否有用？待确认……*

**评价指标**：

- WCSS（Sum of Squares of Within-Clusters）

- 轮廓系数：b为样本点距离最近簇所有点的距离；a为样本点与同簇所有点的距离；s的取值范围为(-1,1)，接近1说明该点离最近簇也很遥远，接近0说明该点离最近簇接近，为负说明该点大概分错了。![image-20211117000836766](https://github.com/XinyueYu16/Machine_Learning/blob/master/assets/Kmeans1.png)
  $$
  s_i = (b_i - a_i)/max(b_i,a_i)
  $$

**风险**：开始点随机，结果随机

- max_iter: 多iter，选择最优

**数据处理**：如果想每个feature的权重一致，先标准化。

**参数选择**：

- **WCCS - Elbow Rule**: A balance between best silhoutte score and limited number of clusters.
- [**S-Score**](https://scikit-learn.org/stable/auto_examples/cluster/plot_kmeans_silhouette_analysis.html#sphx-glr-auto-examples-cluster-plot-kmeans-silhouette-analysis-py): 查看各组s-score是否有负；Elbow Rule.

**模型优化目标**：

 - 直到选择centroid时，簇内点不再重新分配
 - NP-hard: 
    - The average complexity is given by **O(k n T)**, where n is the number of samples and T is the number of iteration.
    - The worst case complexity is given by **O(n^(k+2/p)) with n = n_samples, p = n_features.** (D. Arthur and S. Vassilvitskii, ‘How slow is the k-means method?’ SoCG2006)

**过程**：略

**常用包**：sklearn.cluster.KMeans

- 细节：init默认Kmeans++; 可以输入observation_weight; **如果max_iter过少导致模型在fully_converging前停止, .labels\_和.cluster\_centers\_会对不上**
- KMeans++: 快速、准确

![image-20211117001451772](https://github.com/XinyueYu16/Machine_Learning/blob/master/assets/Kmeans2.png)

**参考**：

- [Visual explanation of KMeans Process](https://www.kaggle.com/shrutimechlearn/step-by-step-kmeans-explained-in-detail)
- [Kmeans++](https://scikit-learn.org/stable/auto_examples/text/plot_document_clustering.html#sphx-glr-auto-examples-text-plot-document-clustering-py)
- [sklearn.cluster.KMeans](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)



Willa

2021.11.16
