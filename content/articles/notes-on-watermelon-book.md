---
title: 机器学习周志华笔记
date: 2026-10-22
---

## 3. 线性模型

### 3.2 线性回归

$y=\omega^Tx+b$

对数线性回归 $\ln y =\omega^Tx+b$。对数函数将线性回归模型预测值和真实标记联系起来。一般地，考虑单调可微函数 $g()$，令 $y=g^{-1}(\omega^Tx+b)$，这样得到的模型称为 GLM，$g()$ 称为联系函数。（$g(\mu)$ 将 $\mu$ 掰到无约束实数轴上以让线性模型去拟合，它不连接 $y$ 本身，连的是 $E(y)$）

### 3.3 对数几率回归 logistic regression

应对分类任务，输出标记 $y\in{0, 1}$，把线性回归模型产生的预测值转化为 0/1 值。所以用对数几率函数：$y=\frac{1}{1+e^{-z}}$ 是一种 sigmoid 函数（S 形）。

代入得出 $y=\frac{1}{1+e^{-(\omega^Tx+b)}}$，变形 $\ln\frac{y}{1-y}=\omega^Tx+b$。将 $y$ 视为 $x$ 作为正例的可能性，与反例的比值 $\frac{y}{1-y}$ 称为几率（odds）。取对数得到对数几率 logit。由此看出实际上是用线性回归模型的预测结果去逼近真实标记的对数几率，得名 logistic regression（并不是逻辑的意思）。

有 $p(y=1|x) = \frac{e^{\omega^T x+b}}{1+e^{\omega^T x+b}}$（$p_1$ 是sigmoid！），$p(y=0|x)=\frac{1}{1+e^{\omega^T x+b}}$。于是可通过最大似然法估计参数，最大化对数似然：$\mathcal{l}(\omega, b)=\sum_{i=1}^m \ln p(y_i | x_i;\omega, b)$。

简写，令 $\beta=(\omega; b)$, $\^x=(x, 1)$，重写为 $p(y_i|x_i; \omega, b) = y_ip_1(\^x_i; \beta)+(1-y_i)p_0(\^x_i;\beta)$。

最大化对数似然等价于最小化（原式加负号）：$\mathcal{l}(\beta)=\sum_{i=1}^m(-y_i\beta^T\^x_i+\ln(1+e^{\beta^T\^x_i}))$。这是一个关于 $\beta$ 的高阶可导连续凸函数。$\beta^*=\argmin_\beta \mathcal{l}(\beta)$。可用梯度下降或牛顿法求最优解。

> 计算技巧：关于 sigmoid 的导数
>
> $\sigma(z)=\frac{1}{1+e^{-z}}$，$\sigma'(z)=\sigma(z)(1-\sigma(z))$。

### 3.4 线性判别分析 LDA

思想：把样例投影到一条直线上，使得同类样例的投影点尽可能接近、异类尽可能远离。

令 $X_i, \mu_i, \sum_i$ 表示第 $i\in{0,1}$ 类示例的集合、均值向量、协方差矩阵。投影到直线 $\omega$ 上，则两类样本中心在直线上的投影分别为 $\omega^T\mu_0$ 和 $\omega^T\mu_1$（标量，直线上的坐标）；协方差分别为 $\omega^T\sum_0\omega,\omega^T\sum_1\omega$。这四个量均为实数！

> 点积 $\omega^Tx=|\omega||x|\cos\theta$，它就代表 $x$ 在直线 $\omega$ 上的坐标

最大化的目标：

$$J=\frac{||\omega^T\mu_0-\omega^T\mu_1||^2_2}{\omega^T\sum_0\omega+\omega^T\sum_1\omega} \\
=\frac{\omega^T(\mu_0-\mu_1)(\mu_0-\mu_1)^T\omega}{\omega^T(\sum_0+\sum_1)\omega}$$

定义类内散度矩阵 $S_\omega=\sum_0+\sum_1=\sum_{x\in X_0}(x-\mu_0)(x-\mu_0)^T+\sum_{x\in X_1}(x-\mu_1)(x-\mu_1)^T$

类间散度矩阵 $S_b=(\mu_0-\mu_1)(\mu_0-\mu_1)^T$

则 $J=\frac{\omega^TS_b\omega}{\omega^TS_\omega\omega}$，称为 $S_b$ 和 $S_\omega$ 的广义瑞利商。

> 一步步推：投影后的偏差 $y-\overline{y}=\omega^T(x-\mu_i)$，平方 $\omega^T(x-\mu_i)(x-\mu_i)^T\omega$（标量的平方有多种写法，但是这里目标是凑成两个 $\omega$ 中间夹一个矩阵的三明治形式（二次型；凑广义瑞利商），标量✖️自身转置=外积三明治。好处是散布矩阵将所有方向的类内方差信息压缩在一个矩阵，只需通过向量 $\omega$ 进行二次型夹心，就算出任意投影方向的方差，也就是以后换一个 $\omega$ 照样算！），对所有样本求和得 $\omega^T[\sum_x(x-\mu_i)(x-\mu_i)^T]\omega$（中间的是各个样本外积形成矩阵之和）

如何确定 $\omega$？注意到式子分子分母都是关于 $\omega$ 的二次项，因此与 $\omega$ 长度无关，只与其方向有关。令 $\omega^TS_\omega\omega=1$，等价于求 $\min\limits_{\omega} -\omega^TS_b\omega \\ s.t. \ \omega^TS_\omega\omega=1$。拉格朗日乘子法 $S_b\omega=\lambda S_\omega\omega$，注意到 $S_b\omega$ 的方向恒为 $\mu_0-\mu_1$（$S_b\omega=\mu(\mu^T\omega)=(\mu^T\omega)\mu$，前面是标量），不妨令 $S_b\omega=\lambda (\mu_0-\mu_1)$，得 $\omega=S_\omega^{-1}(\mu_0-\mu_1)$。实践中进行奇异值分解再求逆。

### TODO: 多分类、类别不平衡

## 4. 决策树

决策树的生成是递归过程。

### 4.2 划分选择

如何选择最优划分属性是学习的关键。我们希望分支节点的纯度尽可能高。

信息熵（information entropy）定义为：$\text{Ent}(D)=-\sum^{n}_{k=1}p_k\log_2p_k$。值越小，混乱度越小，纯度越高。注意到 $-\log_2p_k$：概率越小的事情发生，惊讶度越大，但由于权重小，贡献也小。

假定属性 $a$ 有 $V$ 个可能取值 $\{a^1, ..., a^V\}$，使用该属性对样本集 $D$ 进行划分就产生 $V$ 个分支节点，第 $v$ 个分支节点包含了 $D$ 中所有在属性 $a$ 上取值 $a^v$ 的样本，记为 $D^v$。计算出 $D^v$ 的信息熵在根据所含样本数加权计算出 $a$ 对样本集 $D$ 进行划分所获得的信息增益 information gain：（提问前的迷茫-提问后的平均迷茫，纯度的增加值）

$$\text{Gain}=\text{Ent}(D)-\sum^V_{v=1}\frac{|D^v|}{|D|}\text{Ent}(D^v)$$

可以贪心选择信息增益最大的属性来划分。

问题是，信息增益准则对可取值数目较多的属性有所偏好（比如“编号属性”，每个样本分一类，纯度最高）。考虑用增益率：

$$\text{Gain\_ratio(D, a)}=\frac{\text{Gain}(D, a)}{\text{IV}(a)}$$

$$\text{IV}(a)=-\sum^V_{v=1}\frac{|D^v|}{|D|}\log_2\frac{|D^v|}{|D|}$$

称为 $a$ 的固有值，$a$ 的可能取值数目越多，$IV(a)$ 通常越大。（形式上也是一种熵，衡量“一个样本会掉进哪个分支”这件事本身有多不确定。）

---

CART（Classification and Regression Tree）决策树用基尼指数。从数据集随机抽两个样本（有放回），它们类别不同的概率就是基尼值。

$$\text{Gini}(D)=\sum^n_{k=1}\sum_{k'\ne k}p_kp_{k'} \\ =1-\sum^n_{k=1}p_k^2$$

属性 $a$ 的基尼指数定义为：

$$\text{Gini\_index}(D, a)=\sum^V_{v=1}\frac{|D^v|}{|D|}\text{Gini}(D^v)$$

（$k$ 是样本类别，$v$ 对应属性值）

### 4.3 剪枝 pruning

预剪枝

后剪枝

### 4.4 连续与缺失值

连续属性离散化，最简单的策略是用二分法进行处理。假定属性 $a$ 在 $D$ 上出现了 $n$ 个不同的取值，从小到大排序，可以考察包含 $n-1$ 个元素的候选划分点集合，其中每个划分点是相邻两个 $a$ 取值的均值。然后像离散属性值一样考察划分点，选取最优划分点。与离散属性不同，若当前节点划分属性为连续属性，该属性还可以作为其后代节点的划分属性。

缺失值处理。

1. 如何在属性值缺失的情况下进行划分属性的选择？
   
   给定训练集 $D$ 和属性 $a$，令 $\~D$ 表示 $D$ 中在属性 $a$ 上没有缺失值的样本子集。假定 $a$ 有 $V$ 个可取值，令 $\~D^v$ 表示 $\~D$ 中在属性 $a$ 上取值为 $a^v$ 的样本子集，$\~D_k$ 表示 $\~D$ 中属于第 $k$ 类的样本子集，$\~D=\cup^n_{k=1}\~D_k=\cup^V_{v=1}\~D^v$。假定为每个样本 $x$ 赋一个权重 $w_x$，定义

   $$\rho=\frac{\sum_{x\in\~D}w_x}{\sum_{x\in D}w_x} \\ \~p_k=\frac{\sum_{x\in\~D_k}w_x}{\sum_{x\in\~D}w_x} \\ \~r_v=\frac{\sum_{x\in\~D^v}w_x}{\sum_{x\in\~D}w_x}$$

   分别表示无缺失值样本所占比例、无缺失值样本中第 $k$ 类占比、无缺失值样本中取值 $a^v$ 的样本占比。

   $$\text{Gain}(D,a)=\rho \times \text{Gain}(\~D, a)\\ =\rho\times(\text{Ent}(\~D)-\sum^V_{v=1}\~r_v\text{Ent}(\~D^v)) $$

2. 给定划分属性，若样本在该属性上的值缺失，如何划分？

    将 $x$ 同时划入所有子节点，调整样本权值为 $\~r_vw_x$。直观看就是把一个样本以不同概率划入不同子节点中去。