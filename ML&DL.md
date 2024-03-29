Mathematics
-----------

### Calculus

1.  微分矢量公式涉及梯度、散度、旋度的关系，推导较为简单
2.  雅克比矩阵的每一行为向量值一个分量的梯度

### Linear Algebra

1.  `none`

### Probability & Statistics

1.  `none`

Linear Regression
-----------------

### 技巧

1.  特征归一化可以使不同特征的范围较为接近，从而加快收敛速度

logistic regression
-------------------

### 作用

1.  `logistic`函数即$f(x)=\frac{1}{1+e^{-x}}$，可以将自变量映射到$(0,1)$区间内，常用于聚类算法

### 损失函数

1.  一般使用交叉熵函数
2.  以$f(\vec{x})=\frac{1}{1+e^{-\vec{\theta}^T\vec{x}}}$作为模型时，交叉熵函数相比平方平均函数作为损失函数的最外层有两点优势，其一为梯度中不含激活函数的导数项，可以保证较快的收敛速度，其二为可以保证损失函数永远处处为凸（保证二阶偏导矩阵处处半正定）

regularization parameter
------------------------

### 作用

1.  作为在代价函数中模型参数大小的惩罚的参数（例如参数平方和的系数），用于权衡拟合程度
2.  过拟合一般会出现较大参数，调大`regularization parameter`从而防止过拟合，但`regularization parameter`过大会导致所有参数集中于`0`附近，引起欠拟合
3.  只在`train`过程中使用，在`cross validation`和`test`时不使用

### 简单原理

1. 以`tanh`激活函数为例，当正则化参数越大，则权重越小，激活函数接受的自变量就越小，则其映射效果就越接近线性，最终使得模型不致太过复杂，实现防止过拟合的功能
2. 对于使用`dropout`的正则化，由于有随机失活的存在，会使得权重尽量避免集中在某几个经元上，而是较为均匀的分布在整个网络中，从而使得`L2`范数下降，实现了正则化
3. 对于使用`early stopping`的正则化，可以在`w`值较小时提前结束训练，实现了正则化

Neural Network General
----------------------

### Basic

1.  通过多层网络，实现对现实中复杂函数的模拟
2.  一个`batch`对应一次梯度下降，一个`epoch`对应遍历完一次训练集
3.  `train set`用于训练模型（本质为优化参数），`cross validation set`用于筛选最优模型（本质为优化超参数），`test set`用于评估泛化误差
4.  验证集和测试集的数据分布要一致，不然交叉验证就失去了意义
5.  测试集需要使用训练集的均值和方差来进行归一化
6.  随着数据规模的增大，一般验证集和测试集的比例会下降
7.  `train error`和`cross validation error`接近，说明没有发生过拟合  
8.  残差网络中的残差连接是将某一层激活之后的输出，作为之后某一层激活之前的输入的一部分

### BP Algorithm

1.  其推导推导/计算过程为，先计算除输入层外损失函数对每一层神经元接受的输入值的偏导数，可以写成一个列向量（这也是真正使用反向传播计算的量），然后显然有损失函数对任意一个权重的偏导数等于权重边指向的神经元的输入值的偏导数乘上权重边起始的神经元的输出值
2.  考虑正则化参数时要加上正则化参数项的偏导数
3.  对于`batch size`不为`1`时，要计算的损失函数值为平均值，公式有相应修改
4.  根据`BP`的公式，初始的权重要随机初始化，否则会造成同一层的神经元出现完全相同的变化
5.  需要进行梯度检查验证`BP`的正确性
6.  使用`Relu`函数相比使用`Sigmoid`函数在梯度下降时速度更快（由于函数在自变量大于`0`时导数恒为`1`，不会随自变量增大而下降）
7.  由于梯度消失和梯度爆炸的存在，神经网络的权值进行随机初始化时需要调整方差（当均值已经调整为`0`后即调整`Frobenius`范数的大小），可参考`Xavier`初始化和`He`初始化
8.  数值求梯度进行检验`BP`正确性时，使用双边误差的误差更小
9.  一些优化器（如`Adam`）使用了指数加权平均，相比随机梯度下降等方法，一阶平均的加入使得本次下降值与过去一段时间的下降值的加权平均有关（可以形象理解为“惯性”），抑制了梯度的“摆动”，加速模型收敛，二阶平均的加入使得下降速度受到了负反馈调节

### Normalization

1. `batch normalization`可以不使用偏置值，因为其作用会被抵消
2. `batch normalization`使得神经网络中每层的输入的分布保持一定程度的一致，可以使每一层不太依赖于上一层，进行相对独立的学习，可以得到更好的效果
3. `batch normalization`有轻微的正则化效果，源于其作用于`batch`，相当于给整个训练集增加了一些噪声
4. 测试时的`batch normalization`的均值和方差可以使用训练时的均值和方差的指数加权平均

Error Analysis
----------------------

### Methods

1. 在交叉验证过程中进行误差分析
2. 偏斜类指不同结果类别之间实际出现的频率差异过大，导致不能简单的用总体预测准确率来评估模型的预测效果，所以需要利用查准率（真阳性/(真阳性+假阳性)）和召回率（真阳性/(真阳性+假阴性)）来评估
3. 权衡查准率和召回率用调和平均值较为合理（也被称作`F`函数）

Classic Algorithms
----------------------

### SVM

1. `SVM`通过调整激活函数，可以得到更加鲁棒的分类效果，即将正负样本尽量以大间距分开
2. 核函数可用来提取更多、更本质的特征，适用于样本数多于特征数的情况

### K-means

1. `k-means`用于无监督学习中的聚类算法，需要随机初始化防止落入局部最优，且算法保证迭代过程中损失函数（所有样本点到自身所属（欧式距离最近）的聚类中心点的距离之和）单调下降
2. 在选择合适的聚类种类数时，除了根据实际问题的需求外，`k-means`可以通过肘部（损失函数终态值随聚类种类数的变化曲线突然变“缓”的位置）判断合适的聚类种类数

### PCA

1. `PCA`用于可用于数据降维（压缩），保证投影损失最小
2. `PCA`不适合用于防止过拟合
3. 在原始方法效果不好的情况下再尝试使用`PCA`进行优化

### Anomaly Detection

1. 正样本少且无需区分不同类型的异常时使用异常检测，正样本多或需要对正样本进行区分时采用监督学习

### Collaborative Filtering

1. 协同过滤的一个应用场景是，在应用与用户和产品这一对关系时，可以同时对用户偏好和产品特征进行学习（互为输入与参数的关系）

### Ceiling Analysis

1. 对于一个`AI`系统，通过观察对每一步给出正确的预测结果后，系统最终预测准确率的提升程度，来决定优先优化哪一个环节，一般提升越多越应该先优化

Project
----------------------

### Strategy

1. 可以设立优化指标（尽可能的好）和满足指标（达到就行）来进行项目效果的优化
2. 由于贝叶斯误差无法求得，一般用人类表现最优的误差作为替代标准
3. 修正验证集和测试集上的标签错误比修正训练集上的标签错误更重要，因为前者直接影响对模型效果的评估，后者对训练的结果影响不大
4. 设置`train-dev`数据集（应与`train`数据集来自统同一分布），但不参加训练，和`dev`数据集配合使用，可以用来判断模型表现不够好是因为方差还是因为数据集间的分布不同
5. 使用人工数据合成要注意人工合成的数据的分布相比真实分布过于集中的问题

CNN
----------------------

### Model

1. `inception`网络可以对同时选择的各种大小的卷积核或池化核进行自适应学习
2. 全连接网络是卷积网络的一种特殊形式

### Algorithm

1. 滑动窗口算法不需要显式的分割各个检测方框并分别检测，而是可以仅通过对整个图像进行适当的卷积就可以达到同样的效果，并节省大量重复的计算
2. `yolo`算法可以实现输出可变大小和长宽比例的检测框
3. `anchor box`可以实现同一位置大体形状不同的物体的检测