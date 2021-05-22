# 传统方法

## 协同过滤

### 计算用户相似度(UserCF)

* 余弦相似度: $sim(i,j)=cos(i,j)=\frac{i\cdot j}{||i||\cdot ||j||}$
* 皮尔逊相关系数: $sim(i,j)=\frac{\sum_{p\in P}(R_{i,p}-\overline{R_i})(R_{j,p}-\overline{R_j})}{\sqrt{\sum_{p\in P}(R_{i,p}-\overline{R_i})^2}\sqrt{\sum_{p\in P}(R_{j,p}-\overline{R_j})^2}}$ 减少用户偏置
* $sim(i,j)=\frac{\sum_{p\in P}(R_{i,p}-\overline{R_p})(R_{j,p}-\overline{R_p})}{\sqrt{\sum_{p\in P}(R_{i,p}-\overline{R_i})^2}\sqrt{\sum_{p\in P}(R_{j,p}-\overline{R_j})^2}}$ 减少物品偏置

$R_{u,p}=\frac{\sum_{s\in S}(w_{u,s}\cdot R_{s,p})}{\sum_{s\in S}w_{u,s}}$

$w_{u,s}$是用户$u $和用户$s$的相关系数

### ItemCF

同上,只是对象换了

### 矩阵分解算法

为每个用户和视频生成一个隐向量

最后 $\hat r_{ui}=q_i^Tp_u$

* 奇异值分解: $M=U\Sigma V^T$
* 梯度下降法: $\underset{q*,p*}{min}\sum(\hat r_{ui}-q_i^Tp_u)^2$

打消用户和物品偏差: 

$\hat r_{ui}=\mu +b_i+b_u+q_i^Tp_u$

优点

* 泛化能力强,一定程度上解决了数据稀疏的问题
* 空间复杂度低
* 更好的扩展性和灵活性

### 逻辑回归

$y=sigmoid(x^Tw)$

## FM到FFM自动特征交叉解决方案

### poly2

$PLOY2(w,x)=\sum\sum w_h(j_1,j_2)x_{j_1}x_{j_2}$

### FM

$FM(w,x)=\sum\sum (w_{j_1}w_{j_2})x_{j_1}x_{j_2}$

### FFM

$FM(w,x)=\sum\sum (w_{j_1,f_2}w_{j_2,f_1})x_{j_1}x_{j_2}$

## GBDT+LR

利用训练集训练好GBDT,然后用样本落入GBDT的位置来表示样本的特征

## LS-PLM

利用聚类函数$\pi(x)$来分类增加非线性

$f(x)=\sum \pi(x)\eta(x)=\sum_{i=1}^m\frac{e^{u_i\cdot x}}{\sum_{j=1}^me^{u_j\cdot x}}\cdot \frac{1}{1+e^{-w_i\cdot x}}$

* 端到端的非线性
* 模型的稀疏性强

# 深度学习推荐算法

## AutoRec

一个标准的自编码器

$\underset{\theta}{min}\sum_{r\in S}||r-h(r;\theta)||_2^2$

$h(r;\theta)$为重建函数

## Deep Crossing

### 网络结构

* embedding:对每一项特征进行embedding

* stacking:拼接嵌入特征

* multiple residual units:多层感知机

* scoring:最后的分类和损失函数

## NeuralICF

深度模型协同过滤模型

对用户和物品分别进行embedding,通过内积或神经网络得到得分

## PNN

增强特征交叉能力

相较deep crossing 用 product layer 代替 stacking 从而获得个特征交叉的能力

## wide&deep

当成wide和多层deep组成

wide负责“记忆”(共现频率)

deep负责“泛化”



