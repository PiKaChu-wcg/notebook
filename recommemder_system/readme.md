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

																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																			## deep&cross

稠密特征经过Cross $x_{l+1}=x_0x_l^TW_l+b_l+x_l$

## FM与深度模型的结合

类似deep&cross

$y_{FM}(x):=sigmoid(w_0+\sum_{i=1}^nw_ix_i+\sum_{i=1}^n\sum_{j=i+1}^n<v_i,v_j>x_ix_j)$

使用FM的参数来初始化神经元的初始值

## deepFM

用FM来代替wide部分

## NFM

FM的神经网络化

$y_{NFM}(x):=sigmoid(w_0+\sum_{i=1}^nw_ix_i+f(x))$

## AFM

稀疏输入$\rightarrow$embedding$\rightarrow$ 两两特征交叉$\rightarrow$ 基于注意力机制的池化层$\rightarrow$ 输出层

## 总结

| 模型          | 基本原理                                                     | 特点                                                         | 局限性                                                       |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| AutoRec       | 基于自编码器，对用户或者物品进行编码，利用自编码器的泛化能力进行 | 单隐层神经网络结构简单，可实现快速训练和部署                 | 表达能力较差                                                 |
| Deep Crossing | 利用“Embedding层+多隐层+输出层”的经典深度学习框架，预完成特征的自动深度交又 | 经典的深度学习推荐模型框架                                   | 利用全连接隐层进行特征交叉，针对性不强                       |
| NeuralCF      | 将传统的矩阵分解中用户向量和物品向量的点积操作，换成由神经网络代替的互操作 | 表达能力加强版的矩阵分解模型                                 | 只使用了用户和物品的id特征，没有加入更多其他特征             |
| PNN           | 针对不同特征域之间的交叉操作，定义“内积”“外积”等多种积操作   | 在经典深度学习框架上模型对提高特征交又能力                   | “外积”操作进行了近似化，一定程度上影响了其表达能力           |
| Wide&Deep     | 利用Wide部分加强模型的“记忆能力”，利用Deep部分加强模型的“泛化能力” | 开创了组合模型的构造方法，对深度学习推荐模型的后续发展产生重大影响 | Wide 部分需要人工进行特征组合的筛选                          |
| Deep&Cross    | 用Cross网络替代Wide&Deep模型中的Wide部分                     | 解决了Wide&Deep模型人工组合特征的问题                        | Cross网络的复杂度较高                                        |
| FNN           | 利用FM的参数来初始化深度神经网络的Embedding层参数            | 利用FM初始化参数，加快整个网络的收敛速度                     | 模型的主结构比较简单，没有针对性的特征交叉层                 |
| DeepFM        | 在Wide&Deep模型的基础上，用FM替代原来的线性Wide部分          | 加强了Wide部分的特征交又能力                                 | 与经典的Wide&Deep模型相比，结构差别不明显                    |
| NFM           | 用神经网络代替FM中二阶隐向量交又的操作                       | 相比FM，NFM的表达能力和特征交又能力更强                      | 与PNN模型的结构非常相似                                      |
| AFM           | 在FM的基础上，在二阶隐问量交的基础上对每个交叉结果加入了注意力得分，并使用注意力网络学习注意力得分 | 不同交叉特征的重要性不同                                     | 注意力网络的训练过程比较复杂                                 |
| DIN           | 在传统深度学习推荐模型的基础上引入注意力机制，并利用用户行为历史物品和目标广告物品的相关性计算注意力得分 | 根据目标广告物品的不同，进行更有针对性的推荐                 | 并没有充分利用除“历史行为”以外的其他特征                     |
| DIEN          | 将序列模型与深度学习推荐模型结合，使用序列模型模拟用户的兴趣进化过程 | 序列模型增强了系统对用户兴趣变迁的表达能力，使推荐系统开始考虑时间相关的行为序列中包含的有价值信息 | 序列模型的训练复杂，线上服务的延迟较长，需要进行工程上的优化 |
| DRN           | 将强化学习的思路应用于推荐系统，进行推荐模型的线上实时学习和更新 | 模型对数据实时性的利用能力大大加强                           | 线上部分较复杂,工程实现难度较大                              |

# Embedding

