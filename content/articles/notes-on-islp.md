---
title: Notes on An Introduction to Statistical Learning with Applications in Python
date: 2026-09-20
---

[link](https://www.statlearning.com/)

## 2 Statistical Learning

### 2.1 什么是统计学习

$Y = f(X) + \epsilon$

$f(X)$ 是我们要用模型去估计的目标。随机噪声（误差项 error term） $\epsilon$ 是不可约误差。

预测 Prediction：只关心预测的准确度，不关心过程，模型可以是黑箱

推断 Inference：关心为什么，理解变量与结果之间的关联性

Parametric Method：先假设模型，再拟合（训练）模型

模型假设可能不符。如果用更灵活的模型，引入更多参数，可能会导致**过拟合 overfitting**，把噪声也学习了。

Non-Parametric Method：避免了预设模型严重不符的情况。但是需要更多数据。

---

准确性与模型可解释性之间的 Trade-off。

---

监督学习与无监督学习。

无监督学习没有对应的响应变量。我们可以对观测进行**聚类分析 cluster analysis**，判断各观测可否划分成若干个相对独立的组。

如果只有两个变量，观察平面散点图就能识别聚类。但如果有 $p$ 个变量，不同的散点图就有 $p(p-1)/2$ 幅，需要自动化的聚类方法。

---

变量可以分为定量的和定性的。输出定量变量的叫做回归 Regression，输出定性变量的叫做分类 Classification。基于此选择统计学习的方法。不过输入变量的类型不太重要。

### 2.2 Assessing Model Accuracy

均方误差 mean squared error：$MSE = \frac{1}{n}\sum_{i=1}^n (y_i-\^{f}(x_i))^2$

Training MSE 不重要，重要的是 Test MSE，体现了泛化能力。

模型的灵活性增加，training MSE 会下降，但是 test MSE 不一定。过拟合。

---

**Bias-Variance Trade-off**

expected test MSE：假设有很多训练集，每个训练集都训练出一个模型 $\^f$，用每个模型在同一个测试点 $x_0$ 上预测，计算所有这些预测误差平方的平均值

$E(y_0-\^{f}(x_0))^2=Var(\^{f}(x_0))+[Bias(\^{f}(x_0))]^2+Var(\epsilon)$

期望测试 MSE = 方差 + 偏差² + 不可约误差

- 方差：换一批训练数据，模型预测值的变化程度
- 偏差：模型的平均预测值与真实值差多少

改变模型灵活性会同时影响偏差和方差。

---

前面的概念可以从回归迁移到分类。

error rate：$\frac{1}{n}\sum_{i=1}^n I(y_i\ne \^y_i)$

**贝叶斯分类器**是分类问题的理论最优解：对每个新观测 $x_0$，计算它属于各个类别的条件概率 $P(Y=j|X=x_0)$，然后分给概率最大的类别。它的错误率（贝叶斯错误率）是分类问题的理论下限，现实中无法真正实现，但可以作为衡量其他方法的基准。

---

**K-Nearest Neighbors**

现实生活中我们得不出那个条件概率，只能采用估计的方法。KNN：在训练数据中选出离 $x_0$ 最近的 $K$ 个点，记为 $\mathcal{N_0}$。然后用这些点估计 $j$ 类别的条件概率：$P(Y=j|X=x_0)=\frac{1}{K}\sum_{i\in \mathcal{N_0}}I(y_i=j)$

K 过小，更灵活，太不规则，容易过拟合。K 过大，太平滑，可能欠拟合。

## 3 Linear Regression

$\^y = \^\beta_0 + \^\beta_1 x$

最小二乘法。

$\^\beta_1 = \frac{\sum_{i=1}^n (x_i-\bar x)(y_i-\bar y)}{\sum_{i=1}^n(x_i-\bar x)^2}$

标准误 = 估计量的标准差。

$\text{Var}(\hat{\mu}) = \text{SE}(\hat{\mu})^2 = \frac{\sigma^2}{n}$

标准误 ≈ 单次估计距离真实值的平均偏差。

> 对一个总体多次抽样，每次样本大小都为 n，那么每个样本都有自己的平均值，这些平均值的标准差叫做标准误。

线性回归中，$\text{SE}(\^\beta_1) = \sigma/\sqrt{\sum(x_i-\bar x)^2}$。噪声越小，$x$ 越分散，样本越多，系数估计越准。标准误是置信区间和假设检验的基础，是衡量估计精度的核心指标。

$\sigma ^2$ 未知时用 RSE 估计。（TODO：这段没看。）

### 3.2 Multiple Linear Regression

简单线性回归只能处理 1 个预测变量，但现实中往往有多个。 一个"看起来合理"的做法是分别做多个简单回归，但这样忽略了变量之间的关系，无法区分独立贡献，可能产生误导。多元线性回归的核心价值是：同时考虑所有变量，每个系数表示"控制其他变量后，该变量的净效应"。

$Y=\beta_0+\beta_1X_1+\beta_2X_2 + ... +\beta_pX_p+\epsilon$

---

F 检验回答"所有预测变量整体上有没有用"。

零假设 $H_0$：所有预测变量都不影响 $Y$。

t 值：$t=\frac{\^\beta_1}{\text{SE}(\^\beta_1)}$ ，系数的大小/系数的不确定性。衡量信号相对于噪声的强度。

p 值 = 在零假设 $H_0$ 为真的前提下，观测到当前 t 值的概率。$p=P(|T|\ge |t| | H_0 为真)$。假设无关，看到这种数据的概率。p 值越小，越有理由拒绝 $H_0$。

> TODO：p 值、t 值、R 方需要看书巩固。

选择变量的问题。

模型拟合得好不好的问题。

置信区间 Confidence interval：关心平均值。预测区间 Prediction interval：关心个体值。预测区间比置信区间宽，因为它既要考虑可约误差（估计 $f(X)$ 的误差）和不可约误差（个体点偏离 $f(X)$ 的噪声）

> TODO: ESS、RSS、TSS 未学。DS 感性理解：TSS = 从起点到终点的总距离；ESS = 从起点到模型预测的距离；RSS = 从模型预测到终点的距离。$R^2=\text{ESS}/\text{TSS}$。

### 3.3 Other Considerations in the Regression Model

处理只有两类的定性预测变量，使用独热编码 one-hot encoding：用虚拟变量（Dummy Variables），比如 `own` 值为 1 时表示拥有，0 表示相反。

如果不止两类，需要建立多个虚拟变量。虚拟变量的个数比类别数少一。系数和 $p$ 值会受到编码方式影响。需要用 F 检验整体检验变量。

---

使用线性回归的两个假设：

1. Addictive：每个变量的效果可以加起来，不互相作用
2. Linear：变化率恒定

抛开可加性假设，考虑协同效应 Synergy effect（或 Interaction effect）。可以引入交互项 Interaction term，重新建模：

$$
Y = \beta_0+\beta_1X_1+\beta_2X_2+\beta_3X_1X_2+\epsilon \\
  = \beta_0+(\beta_1+\beta_3X_2)X_1+\beta_2X_2+\epsilon
$$

第二行可以体现：$X_2$ 的改变会影响 $X_1$ 和 $Y$ 之间的关系。1、2 换过来也一样。

即使主效应不显著而交互项显著，主效应也应该保留。这是层级原则 hierarchical principle。

再考虑非线性关系。用多项式回归 Polynomial Regression。可以对 $X$ 做变换，加入新的项，如平方项...

$$
\text{mpg} = \beta_0 + \beta_1 \times \text{horsepower} + \beta_2 \times \text{horsepower}^2 + \epsilon
$$

这仍然是线性模型！线性模型的定义是**对参数线性**。

---

可能的问题：

1. 非线性。通过残差图来识别非线性关系。随机分布在零左右，没有明显模式的残差图才是好的。
2. 误差项相关。误差项不相关是线性回归的核心假设。极端例子：不小心将原始数据复制了一份，每对观测完全相同，误差项完全相关，导致标准误被严重低估，误以为模型非常显著。常见的情况还有时空序列。最好的解决方案是好的实验设计：随机抽样、随机分组、控制混淆变量。
3. 误差项方差不恒定。例如，方差可能随预测值增大而增大，残差图漏斗形。考虑对 $Y$ 做凹函数变换，如 $\sqrt Y$，压缩大值，减小异方差。
4. 离群点 Outliner。$Y$ 异常。用学生化残差检测。
5. 高杠杆点。$X$ 很极端。它对回归线的影响比较大（所以叫杠杆）。需要计算杠杆值。
6. 共线性。两个或多个预测变量高度相关。共线性不会影响预测准确度，但系数估计不精确，会严重影响推断（系数解释和显著性）。VIF 检测。

### 3.5 Comparison of Linear Regression with K-Nearest Neighbors

参数化方法 parametric approach 的优点是易于拟合、易于解释以及检验方便，缺点是如果假设的模型错了会严重偏移。

非参数化方法不显式假设 $f(X)$ 的参数化形式。KNN 回归就是其中一种。

$$
\^f(x_0) = \frac{1}{K} \sum_{x_i\in \mathcal{N_0}} y_i
$$

加入噪声变量后，维度升高时，即使真实关系是非线性的，KNN 效果会严重下滑。而线性回归会自动忽略无用变量，反而更好。根本原因是高维空间中，数据变得稀疏，有效样本量急剧下降。高维空间中所有点都离得很远，局部方法（KNN）失效。因此，在 样本量/维度 大时 KNN 才会更好。

## 4. Classification

分类不能用回归方法的理由：回归方法不能处理两个以上类别的问题、不能提供有意义的概率估计 $P(Y|X)$（预测值不一定在 $[0, 1]$）

### 4.3 Logistic Regression

逻辑回归不直接建模 $Y$，而是预测 $Y$ 属于某类的概率，如 $P(\text{default} = \text{Yes}|\text{balance})$ 表示给定余额，违约的概率。

logistic function：

$$
p(X)=\frac{e^{\beta_0+\beta_1X}}{1+e^{\beta_0+\beta_1X}}
$$

使用极大似然 maximum likelihood 的方法来拟合模型。这种函数（sigmoid）一定会产生介于 0 和 1 之间的 S 形曲线。

$$
\text{odds} = \frac{p(X)}{1-p(X)} = e^{\beta_0+\beta_1X}
$$

几率的范围是 $[0, \infty)$，更适合做数学变换。

$$
\log(\text{odds}) = \beta_0+\beta_1X
$$

对数几率 logit 关于 $X$ 是线性的。

使用极大似然而不是最小二乘法。似然函数可以表示所有样本"预测正确"的概率的乘积，公式如下：

$$
\ell(\beta_0, \beta_1)=\prod_{i:y_i=1}p(x_i)\prod_{i':y_{i'}=0}(1-p(x_{i'}))
$$

实际计算中，通常最大化对数似然。极大似然是一个常见的拟合非线性模型的方法。最小二乘法实际上是它的一个特例。

> 最大似然等价于最小化负对数似然，即 Log Loss（对数损失）。 

---

混杂 confounding：一个外部变量同时影响了自变量和因变量，导致我们误判两者之间的关系。总之，相关不一定表示因果，可能是因为混杂。

---

多分类逻辑回归 Multinomial Logistic Regression。

首先选择一个类别作为基准 baseline，通常选第 $K$ 类。对于 $k = 1, ..., K-1$：$P(Y=k|X=x)=\frac{e^{\beta_{k0}+\beta_{k1}x_1+...+\beta_{k_p}x_p}}{1+\sum_{l=1}^{K-1}e^{\beta_l0+\beta_{l1}x_1+...+\beta{l_p}x_p}}$

对 $K$：$P(Y=K|X=x)=\frac{1}{1+\sum_{l=1}^{K-1}e^{\beta_l0+\beta_{l1}x_1+...+\beta{l_p}x_p}}$

对于 $k=1, ..., K-1$ 有 $\log(\frac{P(Y=k|X=x)}{P(Y=K|X=x)}) = \beta_{k0}+\beta_{k1}x_1+...+\beta_{k_p}x_p$

除了选基准，还可以用 Softmax 编码，对称处理所有类别：

$$
P(Y=k|X=x)=\frac{e^{\beta_{k0}+\beta_{k1}x_1+...+\beta_{kp}x_p}}{\sum_{l=1}^K e^{\beta_{l0}+\beta_{l1}x_1+...+\beta_{lp}x_p}}
$$

任两类之间的 log-odds 仍是线性的：$\log(\frac{P(Y=k|X)}{P(Y=k'|X)})=(\beta_{k0}-\beta_{k'0}) + (\beta_{k1}-\beta_{k'1})x_1 + ...$

### 4.4 Generative Models for Classification

反过来，我们建模 $P(X|Y)$，再用贝叶斯定理转换成 $P(Y|X)$。为什么要用生成式模型？理由如下：

1. 两类完全分离时，逻辑回归系数估计趋于无穷，不稳定
2. 小样本+正态分布更准确
3. 自然扩展到多分类

构建方法：

1. 建模每个类别的先验概率 $\pi_k = P(Y=k)$（还没看 $X$ 时，对 $Y$ 的猜测）
2. 建模每个类别内 $X$ 的分布，密度函数 $f_k(x)=P(X|Y=k)$ （如果 $X$ 离散即分类变量，$f_k(x)$ 是概率。连续时，$f_k(x)$ 是密度，乘 $dx$ 才是概率）
3. 用贝叶斯定理求后验 $P(Y=k|X=x)=\frac{\pi_k f_k(x)}{\sum_{l=1}^K \pi_l f_l(x)}$，记作 $p_k(x)$

难点在于求密度函数。

---

Linear discriminant analysis 线性判别分析

先假设 $p=1$。假设 $f_k(x)$ 服从正态分布。正态密度函数：

$$
f_k(x)=\frac{1}{\sqrt{2\pi}\sigma_k} \exp(-\frac{1}{2\sigma_k^2}(x-\mu_k)^2)
$$

继续假设 $\sigma_1^2 = ... = \sigma_K^2$，简写为 $\sigma^2$。

$$
p_k(x)=\frac{\pi_k\frac{1}{\sqrt{2\pi}\sigma}exp(-\frac{1}{2\sigma^2}(x-\mu_k)^2)}{\sum_{l=1}^K\pi_l\frac{1}{\sqrt{2\pi}\sigma}\exp(-\frac{1}{2\sigma^2}(x-\mu_l)^2)}
$$

要判断 $x$ 属于哪一类，只需比较 $p_k(x)$。为了更简便（乘积变加法），对 $p_k$ 取对数。

$\delta_k(x)=x\frac{\mu_k}{\sigma^2}-\frac{\mu_k^2}{2\sigma^2}+\log(\pi_k)$

$\delta_k(x)$ 是 $x$ 的线性函数，这就是 LDA 名称的来源。

决策边界：$\delta_1(x)=\delta_2(x)$ 得 $x=\frac{\mu_1^2-\mu_2^2}{2(\mu_1-\mu_2)}=\frac{\mu_1+\mu_2}{2}$，是两个均值的中点。LDA 决策边界与贝叶斯决策边界非常接近（但不完全相等）。因为 LDA 用估计的 $\^\mu_1,\^\mu_2$ 计算边界。

$$
\^\mu_k = \frac{1}{n_k}\sum_{i:y_i=k}x_i \\
\^\sigma^2 = \frac{1}{n-K}\sum_{k=1}^K\sum_{i:y_i=k}(x_i-\^\mu_k)^2
$$

用样本比例估计先验概率：$\^\pi_k=n_k/n$

$\^\delta_k(x)=x\frac{\^\mu_k}{\^\sigma^2}-\frac{\^\mu_k^2}{2\^\sigma^2}+\log(\^\pi_k)$

决策规则：把 $x$ 分给 $\^\delta_k(x)$ 最大的类。

总结：LDA = 估计均值 + 合并方差 + 先验比例，代入线性判别函数，取最大。核心假设包括正态分布、均值不同、方差相同。

---

下面讨论 $p>1$。服从多元高斯分布。对于 $p$ 维向量， $X\sim N(\mu, \Sigma)$。协方差矩阵 $\text{Cov}(X)=\Sigma$。密度函数：$f(x)=\frac{1}{(2\pi)^{p/2}|\Sigma|^{1/2}}\exp(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu))$

判别函数是：

$$
\delta_k(x)=x^T\Sigma^{-1}\mu_k-\frac{1}{2}\mu_k^T\Sigma^{-1}\mu_k+\log\pi_k
$$

判别函数是线性的。关键在 $\Sigma$ 是所有类共享的，意味着每类的等高线椭圆形状完全相同，只是平移了中心。

> 矩阵推导、马氏距离之类的数学推导以后要学。islp 没有讲。

---

以银行客户违约率为例。敏感度 Sensitivity 指真正例率即真正违约的人中识别出多少，特异度 Specificity 指真负例率。高特异度、低敏感度 = 模型宁可漏掉违约者也不误判不违约者。原因是 LDA 的目标是最小化总错误率，平等对待 FN、FP，可能导致敏感度低。因此需要改变阈值，例如改为若 $P(\text{default} = Yes|X)>0.2$ 则预测违约。

ROC 图评估分类器。

---

二次判别分析 Quadratic Discriminant Analysis

与 LDA 不同的是，它假设每个类有自己的方差，即假设 $X \sim N(\mu_k, \Sigma_k)$，$\Sigma_k$ 表示第 $k$ 类的协方差矩阵。注意下面 $x$ 是二次的。

$$
\delta_k(x) = -\frac{1}{2}(x-\mu_k)^T\Sigma_k^{-1}(x-\mu_k)-\frac{1}{2}\log|\Sigma_k|+\log\pi_k \\
 = -\frac{1}{2}x^T\Sigma_k^{-1}x+x^T\Sigma^{-1}_k\mu_k-\frac{1}{2}\mu_k^T\Sigma^{-1}_k\mu_k-\frac{1}{2}\log|\Sigma_k|+\log\pi_k
$$

当训练样本少或方差是关键问题时，用 LDA。样本多或方差明显不同时，用 QDA。**假设越强，需要的数据越少**

---

朴素贝叶斯

上面 LDA、QDA 都是通过正态分布的假设来简化密度函数的求法的。朴素贝叶斯不对变量分布类型做特殊假设，而只是假设**在每个类别内，$p$ 个预测变量相互独立**。即 $f_k(x)=f_{k1}(x_1)\times...\times f_{kp}(x_p)$。$f_k(x)$ 是第 $k$ 类中 $X$ 的联合密度，$f_{kj}(x_j)$ 表示第 $k$ 类中第 $j$ 个变量的边缘密度。估计 $p$ 维密度函数需考虑每个变量的边缘分布和变量间的联合分布（协方差矩阵非对角元素），而假设变量之间独立就可以避开相关性问题，拆成了 $p$ 个一维问题。朴素贝叶斯故意做了一个"错误"的假设，因为它的收益（降方差）大于代价（增偏差）。这又是 bias-variance trade-off 的体现。（参数少，估计更稳定）$p$ 大，$n$ 小时，降方差效果最明显。

$$P(Y=k|X=x)=\frac{\pi_k \times f_{k1}(x_1) \times ... \times f_{kp}(x_p)}{\sum_{l=1}^K \pi_l \times f_{l1}(x_1) \times ... \times f_{lp}(x_p)}$$

估计 $f_{kj}(x_j)$ 的方法：

1. 定量 + 参数化：正态分布假设。与 QDA 的区别在于朴素贝叶斯的协方差矩阵是对角矩阵，非对角线全为零。
2. 定量 + 非参数化：直方图或核密度估计（KDE），需要更多参数。
3. 定性变量：直接数比例。

### 4.5 A Comparison of Classification Methods

把所有方法写成 log-odds 形式再比较。

$$\log(\frac{P(Y=k|X=x)}{P(Y=K|X=x)})$$

### 4.6 Generalized Linear Model

线性回归假设 $Y$ 是连续、正态分布的，但现实中可能是二分类、计数、正数。GLM 保留线性的核心，但允许 $Y$ 服从不同分布。

Poisson Regression 泊松回归

泊松分布：假设 $Y \in \{0, 1, 2, ...\}$。如果 $Y$ 服从泊松分布，则 $P(Y=k)=\frac{e^{-\lambda} \lambda^k}{k!}$。

$\lambda = \text{E}(Y) = \text{Var}(Y)$。$Y$ 的均值越大，方差越大。

泊松分布用来计数。

泊松回归的核心思想：让 $\lambda$ 是 $X$ 的函数：$\lambda(X_1, ..., X_p)$：

$$\log(\lambda(X_1, ..., X_p)) = \beta_0 + \beta_1X_1 + ... + \beta_pX_p$$

为什么要用对数函数连接？如果直接线性建模 $\lambda = \beta_0+\beta_1X$，$\lambda$ 可能为负数。只有设 $\lambda = e^{\beta_0+\beta_1X}$ 才能保证为正。

用最大似然方法来估计系数。似然函数 = 所有样本柏松概率乘积。

$$\ell(\beta_0, ..., \beta_p)=\prod_{i=1}^n \frac{e^{-\lambda(x_i)}\lambda(x_i)^{y_i}}{y_i!}$$

> tips: 似然函数 = 给定数据时，不同参数值"产生"这组数据的可能性。

---

三种回归模型线性、逻辑、柏松回归的共同点：

1. 假设 $Y$ 的分布
2. 建模均值 $E(Y|X)$
3. 连接函数将均值变换到 $(-\infty, +\infty)$ 使其与线性预测子匹配

这些分布都属于指数族 exponential family。GLM 的通用模式：先选分布，再选连接函数变换均值 $E(Y|X)$，然后建模，变换后的均值是 $X$ 的线性函数。

看不懂，先跳了（

## 5. Resampling Methods

重采样指从训练集反复抽取样本，每次重新拟合模型，从多次拟合中获取额外信息。

### 5.1 Cross-Validation

验证集法 Validation Set Approach

把数据随机分成两份：训练集和验证集（validation/hold out set）。在训练集上拟合，在验证集上评估。

缺点：高方差（测试误差高度依赖于这一次的具体划分）、低估模型表现（只用部分数据训练）。两个缺点都源于"只做一次划分 + 只用部分数据"。

---

留一交叉验证 Leave-One-Out Cross-Validation, LOOCV

每次只留一个样本作为验证集，其余全部用于训练。

$\text{CV}_{(n)}=\frac{1}{n}\sum_{i=1}^n \text{MSE}_i$

$\text{MSE}_i = (y_i-\^y_i)^2$，$\^y_i$ 表示用除了 $i$ 样本外所有数据训练，再预测 $i$ 样本的结果

单个 $MSE_i$ 方差很大，但取平均后就小了。

理论上需要拟合 $n$ 次。但是对于最小二乘线性/多项式回归，LOOCV 的计算量 = 一次拟合的计算量。

---

k 折交叉验证 k-Fold Cross-Validation

把数据随机分成 $k$ 份（fold），每次用其中一份做验证，其余做训练。

$$\text{CV}_{(k)}=\frac{1}{k}\sum_{i=1}^k \text{MSE}_i$$

$k=5$ 或 $k=10$ 是经验最优。

---

LOOCV 的方差比 $k$ 折 CV 高，原因是：LOOCV 每次训练集几乎相同（只差一个样本），导致 $n$ 个模型高度相关；平均高度相关的量，方差降低有限。 而 $k$ 折 CV 的 $k$ 个模型训练集重叠少，相关性低，平均后方差小。

### 5.2 Bootstrap

自助法是一种通用工具，用来量化估计量或模型的**不确定性**。

如果能反复从真实分布抽样，就能算出标准误。但现实只有一份数据。自助法的核心是“有放回抽样”：从原始数据中随机抽取 $n$ 个观测（可重复），得到一个自助样本。每个自助样本产生一个估计值。重复 $B$ 次得到多个估计，用它们的标准差作为标准误。

## 6. Linear Model Selection and Regularization

改进线性模型。

替换拟合方法。有两个目标：

1. 预测准确性。最小二乘法在 $n$ 远大于 $p$ 时，表现很好。当 $n$ 接近 $p$ 时，方差大。$p>n$ 时，无唯一解。
2. 模型可解释性。很多变量和 $Y$ 无关却被包含在模型里，难以解释。用变量选择方法（如 Lasso）把无关变量的系数设为 0。

有三类方法：子集选择、收缩（把系数往 0 压缩）、降维。

### 6.1 Subset Selection

Best Subset Selection

对 $p$ 个变量的所有组合分别拟合，选最好的。

算法如下：对每个 $k$ 选最好的模型（RSS 最小），记为 $M_k$。然后再从 $M_0, ..., M_p$ 中选一个。

---

Stepwise Selection

前向逐步选择从空模型开始，每次加一个最好的变量，直到加完。每次选那个"加了之后模型改善最多"的变量。这种贪心策略也有错过“组合起来最好”变量的风险。

后向逐步选择先保留全部变量。同样不保证能够得到最优解。

---

估计测试误差的两种思路：间接估计和直接估计。

间接估计：Cp、AIC、BIC、adjusted $R^2$。间接估计=训练误差+惩罚项。惩罚项与模型复杂度（变量数）有关。

直接估计：交叉验证。

### 6.2 Shrinkage Methods

岭回归 Ridge Regression

最小二乘法的结果是得到让残差平方和最小的参数。而岭回归也类似，它要最小化的是 $\text{RSS}+\lambda \sum_{j=1}^p \beta_j^2$，多了一个惩罚项 shrinkage penalty，让系数"收缩"到接近 0。$\lambda$ 控制拟合和收缩的权衡。

最小二乘法有尺度不变性 scale equivariant，不管单位怎么变，系数都可以相应调整，对变量缩放不敏感。然而岭回归没有尺度不变性，惩罚项依赖于 $\beta_j$ 的大小，惩罚项的力度会变化。如果不**标准化**，岭回归会被尺度大的变量主导。

p 大或 n 小时，岭回归更有优势。

---

Lasso

岭回归的缺点是无论如何它都会包括所有变量。Lasso 和岭回归的区别是平方项改成了绝对值。

$$\text{RSS}+\lambda\sum_{j=1}^p |\beta_j|$$

当 $\lambda$ 足够大时，一些系数会等于零。Lasso 可得到稀疏模型，只有少数系数非 0，使得更容易解释。

### 6.3 Dimension Reduction Methods

令 $Z_1, Z_2, ..., Z_M$ 代表 $M<p$ 个原来 $p$ 个变量的线性组合。

$$Z_m = \sum_{j=1}^p \phi_{jm}X_j$$

然后用新变量做回归：

$$y_i = \theta_0 + \sum_{m=1}^M\theta_m z_{im} + \epsilon_i$$

降维 = 用少量参数捕捉主要信息。如果 $Z_m$ 选得好，降维回归效果会比直接最小二乘更好。

---

Principal Components Regression 主成分回归

第一主成分方向即数据“变化最大”的方向。把每个数据点“投影”到直线上，所有投影值的方差最大，即信息保留最多。还要约束 $\sum \phi^2 = 1$，保证载荷唯一（约束为单位向量）。第二主成分 $Z_2$ 则捕捉 $Z_1$ 没捕捉到的剩余信息，要与其正交。

PCR 简单，但不一定最优。注意 PCR 是“降维工具”，不是“特征选择工具”。它没有丢弃任何原始变量。

---

Partial Least Squares

PCR 是无监督学习，$Y$ 不被纳入考虑。而 PLS 选的方向既保留 $X$ 的信息，又与 $Y$ 有关。

标准化后构造第一 PLS 方向，对每个 $X_j$，单独做 $Y\sim X_j$，取 $\phi_{j1}=$该回归的系数，$Z_1=\sum_{j=1}^p\phi_{j1}X_j$。可见简单回归的系数和 $Y$ 与 $X_j$ 的相关性成正比。在求 $Z_2$ 时，对每个 $X_j$ 做 $X_j\sim Z_1$，取残差（$X_j$ 减去 $Z_1$ 能解释的部分），用残差代替原始 $X_j$ 找 $Z_2$。残差也保证了正交性。

### 6.4 Considerations in High Dimensions

$p > n$ 或接近时，最小二乘法失效。这时本章的方法就有用了。

1. 正则化在高维中至关重要
2. 调参（$λ$）决定预测表现
3. 测试误差随维度增加，除非新变量真的和 $Y$ 相关。Lasso 的自由度 = 非零系数个数，是模型复杂度的度量。

## 7. Moving Beyond Linearity

### 7.1 Polynomial Regression

多项式回归，前面讲过了。

### 7.2 Step Functions

多项式是全局的，改变一个系数，整条曲线都变。使用阶梯函数将 $X$ 的范围切成几段，每段用不同的常数。

### 7.3 Basis Functions

不直接用 $X$ 建模，而是用 $X$ 的变换 $b1(X), ..., b_K(X)$ 建模。

$$y_i = \beta_0+\beta_1 b_1(x_i) + ... + \beta_K b_K(x_i) + \epsilon_i$$

基函数是人选定的。对多项式回归来说，基函数就是 $b_j(x_i)=x_i^j$，阶梯函数就是 $b_j(x)=I(c_j\le x < c_{j+1})$。这提供了一种统一的视角。

### 7.4 Regression Splines

Piecewise Polynomials 

不用一个高次多项式，而是分成几段，每段用低次多项式。

使用更多节点可以使模型更灵活。然而两段多项式在节点处跳跃了，需要加约束让它连续。

Constraints and Splines

我们可以加约束：在节点处连续、一阶导连续、二阶导连续，使它更为光滑。约束会消耗自由度。三次样条 cubic spline = 分段三次多项式 + 结点处连续 + 一阶导连续 + 二阶导连续，自由度 = $4+K$。$d$ 次样条要求 $d-1$ 阶导连续。三次样条“足够光滑”，人眼看不出结点。

一个有 $K$ 个节点的三次样条可以被建模为

$$y_i = \beta_0+\beta_1 b_1(x_i) + ... + \beta_{K+3}b_{K+3}(x_i)+\epsilon_i$$

基函数可以用 $x, x^2, x^3$ 然后再对每个节点加一个截断幂基 truncated power basis，其定义如下：

$$h(x, \xi) = (x-\xi)^3_+ = \begin{cases} 
(x - \xi)^3 & \text{if } x > \xi \\ 
0 & \text{otherwise,} 
\end{cases}$$

这样只在结点处引入不连续的三阶导，保持低阶导连续。

普通样条在边界处（$X$ 很小或很大）方差高、置信带宽、预测不稳定。 自然样条 = 普通样条 + 边界线性约束，在边界外要求函数是直线。效果：边界更稳定、置信带更窄、自由度更少（$K+2$ 而非 $K+4$）。 自然样条是实际中最常用的样条。

Choosing the Number and Locations of the Knots

样条比多项式回归更稳定。因为样条用“结点”引入灵活性，而多项式用“高次幂”引入灵活性。

### 7.5 Smoothing Splines

我们要找的模型既要让 RSS 小，又要平滑，防止过拟合。于是我们尽量让以下尽量小：

$$\sum_{i=1}^n(y_i-g(x_i))^2+\lambda\int g''(t)dt$$

Loss + Penalty 模式。前一半是损失函数，后一半是粗糙度惩罚。二阶导数衡量粗糙度。$\lambda$ 控制拟合和平滑的权衡。

使其最小化的解就是自然三次样条。节点自动放在每个数据点 $x_i$。

$\lambda$ 从小到大，**有效自由度**从 $n$ 到 $2$。

$$\^{g_\lambda} = S_\lambda y$$

$\^{g_\lambda}$ 是 $n$ 维向量，代表每个训练点的拟合值。$S_\lambda$ 是 $n\times n$ 矩阵，平滑矩阵。$y$ 是响应变量。拟合值是 $y$ 的线性变换。有效自由度 $df_\lambda$ 等于平滑矩阵对角元素之和。

### 7.6 Local Regression

对每个预测点 $x_0$，只用它附近的点来拟合。

局部回归和 KNN 有相似之处，不过它是做加权回归。

### 7.7 Generalized Additive Models

从单变量推广到多变量。

$$y_i = \beta_0 + f_1(x_{i1}) + ... + f_p(x_{ip}) + \epsilon_i$$

称之为加性 additive 是因为我们对每个 $X_j$ 分别计算 $f_j$（非线性），然后再把它们的贡献全部加起来。例如：

$$\text{wage}=\beta_0+f_1(\text{year})+f_2(\text{age})+f_3(\text{education})+\epsilon$$

对前两个用自然样条拟合，第三个定性变量匹配常量。自然样条可以看作是基函数的线性组合，因此可以用最小二乘法拟合。把样条函数和虚拟变量拼在一起形成大的设计矩阵，做一次大的线性回归。

平滑样条的 GAM 比较复杂，因其不能直接使用最小二乘。要用 backfitting。

## 8. Tree-Based Methods

### 8.1 The Basics of Decision Trees

回归树

划分区域，取均值。

Recursive Binary Splitting 是一种贪心的方法。每次找当前最优一刀，最小化 RSS，

Prune 剪枝。先长大树再剪枝，避免短视。

Cost Complexity Pruning 代价复杂度剪枝。Loss + Penalty

分类树。不能用 RSS，可以用分类错误率 $E = 1-\max_k(\^p_{mk})$，1 减去最大类别比例。但这个标准不够敏感。可以用 Gini index $G=\sum_{k=1}^K \^p_{mk}(1-\^p_{mk})$，理解为各类别比例的“不纯度”之和。另一种度量是熵 entropy，$D=-\sum_{k=1}^K \^p_{mk}\log\^p_{mk}$。和 Gini 类似，体现不纯度，越小越好。

决策树的预测能力不太好。下面用其他方法提高预测准确率。

### 8.2 Bagging, Random Forests, Boosting, and Bayesian Additive Regression Trees

Bagging = Bootstrap + Aggregation

决策树的方差高，考虑降方差。平均 $n$ 个独立观测，方差可以降低 $1/n$。那么用多个训练集建立多个模型求平均就可以得到低方差模型。但现实中只有一份训练数据，只能用 Bootstrap 生成多个训练集：从原始数据有放回抽取 $B$ 个样本，每个样本建一棵树，再求平均。Bagging 是一个普适的方法，但对决策树特别有效。缺点是牺牲了可解释性，因为有 $B$ 棵树，看不出哪个变量重要。为了度量变量重要性，可以检测 RSS/Gini Index 减少量。

Random Forests

随机森林在 bagging 的基础上，限制每次分裂只考虑一部分变量，大大降低了树之间的相关性。这样做有效的原因是，假设有一个非常强的变量，大多数树都会在第一次分裂时用它，导致这些树同质化，降方差的效果有限。强制只考虑部分变量可以让树更多样，平均降方差效果更好。

Boosting

通过慢慢学避免过拟合。每次对残差建树，用 $d$ 控制叶子数，用收缩参数 $\lambda$ 降低每棵树的贡献，渐进式学习。

Bayesian additive regression trees (BART)

BART 融合了上面两种方法的随机、逐步改进的优点。有 $K$ 棵树，$B$ 次迭代。第 $b$ 次迭代的预测 $\^f^b(x)=\sum_{k=1}^K\^f_k^b(x)$。初始化每棵树只有一个根结点，$\^f_k^1(x)=\frac{1}{nK}\sum_{i=1}^ny_i$。在后续迭代中，一一更新每棵树。在第 $b$ 次迭代更新第 $k$ 棵树时，我们从其他树提取预测值，求部分残差 $r_i=y_i-\sum_{k'<k}\^f_{k'}^b(x_i)-\sum_{k'>k}\^f_{k'}^{b-1}(x_i)$。然后不建新树，而是微调旧树。可以通过加分支、剪分支进行结构扰动，或者改叶子预测值进行数值扰动。扰动是随机的，但偏好能改善的扰动。

## 9. Support Vector Machines

### 9.1 Maximal Margin Classifier

在一个 $p$ 维空间中，超平面是一个维度为 $p-1$ 的平坦仿射（affine，子空间不必过原点）子空间。简单的说超平面就是把一个空间一分为二的刀切面。$p$ 维空间的一个超平面可以定义如下：$\beta_0+\beta_1 X_1+...+\beta_p X_p=0$。

假设有 $n\times p$ 的数据矩阵，包含 $n$ 个 $p$ 维样本，每个样本对应一个类别标签 $y_i \in \{-1, 1\}$。假如 $\beta^Tx_i+\beta_0>0$，$y_i=1$。若小于零，则 $y_i=-1$。我们判定一个样本 $x^*$ 属于哪一类只需看 $f(x^*)$ 的正负号。$f(x^*)$ 的绝对值大小也代表了样本离超平面的远近。可以发现这样只能解决线性可分问题。

Maximal Margin Classifier

如果数据是完美线性可分的（即两类点之间有一条清晰的鸿沟，没有混在一起），那么能分开它们的超平面有无限多个。必须制定一个合理的规则来做出最终决定。最大间隔分类器规定选那个离所有数据点都尽可能远的超平面。Support Vector 就是那些“撑起”分界线的、离分界线最近的关键数据点。最大间隔超平面只依赖于支持向量，完全不依赖其他观测点。

我们要最大化 $M$，同时要求 $\sum_{j=1}^p\beta_j^2=1$，还有满足 $y_i(\beta_0+\beta_1x_{i1}+...+\beta_p x_{ip}) \ge M \ \forall i=1, ..., n$

对最后一个约束，含义是保证分到正确的一侧，而且距离要大于等于 $M$。而第一个约束是“归一化”，使得 $M$ 代表物理距离（$d=\frac{|\beta_0+\beta^Tx^*|}{||\beta||}$，参考点到直线距离公式）。

### 9.2 Support Vector Classifiers

允许错分类几个，可能会让整体更好。而且有些时候数据可能不可分，软间隔是唯一选择。支持向量分类器（软间隔分类器）允许部分点越界。有两种越界：在间隔的错误一侧和在超平面的错误一侧。

优化问题的目标在最大间隔分类器的基础上多了松弛变量，就是变成 $y_i(\beta_0+\beta_1x_{i1}+\beta_2x_{i2}+...+\beta_px_{ip})\ge M(1-\epsilon_i)$，且 $\epsilon_i \ge 0, \sum_{i=1}^n\epsilon_i \le C$。$C$ 控制“软”的程度。如果 $\epsilon_i >0$ 则说明在间隔内，$\epsilon_i>1$ 则说明分类错了。

只有在间隔上或违反间隔的点影响分类器，这些点叫支持向量。远离超平面的点不影响分类器，这体现了稳健性。而 LDA 会依赖全部点求均值，受极端值影响。

### 9.3 Support Vector Machines

支持向量机可以自动将线性分类器转为非线性。一个一般的机制是把数据映射到高维空间，引入 $X^2$ 等等，在新的空间里线性可分。SVM 可以自动地设计映射。

SVM 用 kernels 核函数来扩大空间。注意到支持向量分类器的解只依赖观测之间的内积（内积体现相似度），线性支持向量分类器就可以表示为 $f(x)=\beta_0+\sum_{i=1}^n\alpha_i\langle x, x_i\rangle$，即判别函数是新点和所有训练点内积的加权和。如果把内积换成核函数就能升维。

> 为什么是内积？$\^\beta_j=\sum_{i=1}^n\alpha_ix_{ij}$，每个 $\beta_j$ 都是训练观测的加权和。然后判别函数就可以表示成内积了

计算 $f(x)$，实际上只需要考虑支持向量（其他点 $\alpha_i$ 为 0）。记 $S$ 表示支持向量的索引集合，我们只用计算支持向量的内积：$f(x)=\beta_0+\sum_{i\in S}\alpha_i\langle x, x_i \rangle$，这就大大减少了计算量。

把内积替换成核函数 $K(x_i, x_{i'})$。核函数是指衡量两个观测相似度的函数。内积就是线性核。$K(x_i, x_{i'})=(1+\sum_{j=1}^px_{ij}x_{i'j})^d$ 是多项式核。$K(x_i, x_{i'})=\exp(-\gamma\sum_{j=1}^p(x_{ij}-x_{i'j})^2)$ 是径向核，衡量距离相似度，只有附近的点有影响（指数衰减极快），是一种**局部核**。

为什么不直接把 $X^2, X^3$ 加入模型中而要计算核函数？核函数只计算 $n$ 个点两两之间的核值，不进入高维空间，避免了显式升维的计算量。

逻辑回归与 SVM 的相似性。它们都是 Loss+Penalty，只是损失函数不同。分离好的情况用 SVM，重叠多用逻辑回归。SVM 的损失函数在在极远处为 $0$，而逻辑回归只会接近但不等于零。

## 10. Deep Learning

### 10.1 Single Layer Neural Networks

