# Feature Importance

给客户做模型的时候，客户最常见要求的就是解释模型，比如：

- 为什么这个房价这么高，这么低？
- 假设我拿这些数据去量化新的项目，比如距离地铁站距离1km，是不是以同样的规律预测房价？



根据我查到的资料，有两种feature importance，一种是**模型层面**的，简约理解为这个特征在训练模型的过程中有多重要；另一种是**样本层面**的，理解为这个项目有这样的结果的原因，是由哪些特征决定的，哪些更重要。



#### 模型层面

1. Sklearn: feature_importance：基于sklearn模型（主要为树模型）的分叉规则，特征重要性与该特征在节点分支判断上出现的次数呈正比
2. Permutation importance：将一个特征打乱后，给模型带来的影响
   - sklearn中的permutation_importance既可以计算train，又可以计算test中的feature importance
   - 相比于自带的feature_importance而言，permutation_importance对于分类变量的打分会更友好
3. Drop importance：将一个特征去掉后，给模型带来的影响

- *一个猜想：有没有可能出现2/3的影响不大而1的影响大，如果是，是不是可以用来说明有共线性？*
- 一种实践：教程里单独创建了一列random数作为特征，在模型中可以将关注特征和random特征的重要性进行对比，看是否真的重要



#### 样本层面

1. Tree Interpretation：一个基于树的包，从样本所在叶节点往上回溯，看遇到的哪些支线任务给自己加了多少属性（。
2. LIME：暂时没弄完全明白，是根据样本和模型*局部线性拟合*，推测的特征重要性。不拘泥于生产结果的模型。



Willa

2021.11.28

