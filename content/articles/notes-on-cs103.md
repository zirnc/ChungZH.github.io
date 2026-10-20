---
title: Notes on CS103 Mathematical Foundations of Computing
date: 2026-09-19
---

# Notes on _CS103: Mathematical Foundations of Computing_

[🔗link](https://web.stanford.edu/class/archive/cs/cs103/cs103.1268/)

## 0.Set Theory and Introduction to Proofs

### Introduction to Set Theory

**势（Cardinality）**：反映集合的大小。

定义 $\aleph_0 = |\mathbb{N}|$（可数无穷）。

如果两个集合存在双射，就称这两个集合等势。

自然数集和偶数集是等势的，只需将 n 与 2n 一一对应。

---

**康托定理**：$|A| < |\wp(A)|$。

对角线证明法：先假设 $S$ 与它的幂集能一一配对（$s_n$ 配对子集 $A_n$）。构造一个新的子集 $D$，规则是：

对于每一个元素 $s_n$，若 $s_n \not\in A_n$，就把 $s_n$ 放入 $D$。

可以证明 $D$ 一定不在表里。

---

由于程序的代码一定是字符串，得 $|Programs| \leq |Strings|$。

一个字符串集合可以对应一个判定问题，如给出一个集合 $S = \{"a", "b", ..., "z"\}$，它对应一个问题：给一个字符串，判定是不是一个小写字母。所以，由字符串集合定义的问题，和字符串集合本身，是一一对应的。但是，问题绝不仅有判定问题。因此得到 $|Sets of Strings| \leq |Problems|$

得到：$|Programs| \leq |Strings| \leq |\wp(Strings)| \leq |Problems|$

结论：程序作为有限字符串是可数的，而很多我们想计算的对象是不可数的，因此必然存在大量不可计算、不可判定的东西。

### Mathematical Proofs

三要素：definitions、intuitions、conventions。

## 1.More Applications of Proofs, plus Negations

这里比较陌生的是逆否证法：一个命题（若 P 则 Q）与其逆否命题（若非 Q 则非 P）在逻辑上是等价的，证明其中一个就等于证明了另一个。

## 2.Propositional Logic and Existential & Universal Quantifiers

### Propositional Logic 命题逻辑

命题（Proposition）是一个本身要么为真、要么为假的陈述。

命题逻辑是一种数学系统，用于对命题及其相互关系进行推理。命题逻辑中的每个陈述，都是由命题变元（一个命题，用 $p, q, r$ 表示）通过命题联结词组合而成的。

Mathematical Implication 数学蕴含：$p \to q$

Material Implication 实质蕴涵。

|$p$|$q$|$p\to q$|
|--|--|--|
|F|F|T|
|F|T|T|
|T|F|F|
|T|T|T|

- 空真 vacuously true：前件（antecedent）为假，这个蕴含自动为真
- 平凡真 trivially true：后件（consequent）为真，前件无所谓了

> DS：你可能会觉得“假前件推出真命题”很奇怪。但这样做有一个巨大的好处：推理规则变得简单且一致。
>
> 比如，我们要证明“对所有 $n$，如果 $n$ 是偶数，那么 $n^2$ 是偶数”。
>
> - 当 $n$ 是偶数时，前件真，我们需要证明后件也真。
> - 当 $n$ 是奇数时，前件假，按照定义，这个蕴涵自动为真，我们不用管它。
>
> 如果没有“空真”这个规定，那么对于“如果 $n$ 是偶数……”这样的命题，我们就还得额外讨论 $n$ 是奇数时该怎么办，数学证明会变得非常繁琐。
>
> 所以，这个规定不是逻辑上的“漏洞”，而是为了让整个数学体系能够顺畅运转而刻意设计的。

### Propositional Equivalence

- **de Morgan's Laws**: $\neg(p\land q)$ 和 $\neg p \lor \neg q$、$\neg(p\lor q)$ 和 $\neg p \land \neg q$ 是等价的

  不要写 `if (!(p() && q()))`，而要写 `if (!p() || !q())`，提高效率
- **蕴涵否定**：$p \to q$ 等价于 $\neg p \lor q$

  把难以直接实现的“蕴涵”，翻译成了计算机和电路唯一能懂的“与、或、非”。

### First-Order Logic

一阶逻辑是一个用于推理对象属性的逻辑系统。它在命题逻辑的逻辑联结词基础上，增加了：

- 谓词 predicates：用于描述对象的属性
- 函数 functions：用于将一个对象映射到另一个对象
- 量词 quantifiers：允许我们对多个对象进行推理

将一个谓词应用于参数（自变量），会产生一个命题，该命题要么为真，要么为假。

## 3.More Propositional Logic and Functions

- $\neg (p \land q)$ 等同于 $p \to \neg q$
- $\neg (p \to q)$ 等同于 $p \land \neg q$

推荐将 $\to$ 与 $\forall$、$\land$ 和 $\exists$ 配套使用。

## 4.Functions and Graphs

### Graph Theory

A graph consists of a set of nodes connected by edges.

Graph: undirected graph

Digraph: directed graph

一般在有向图中，自环是允许的。

点覆盖（vertex cover）：对 $G=(V, E)$，一个点覆盖是满足以下条件的集合 $C$：$\forall x \in V. \forall y \in V. (\{x, y\} \in E \to (x\in C \lor y \in C))$。

独立集（independent set）：任意两点不相邻

顶点覆盖的补集，必然是独立集；独立集的补集，必然是顶点覆盖。

## 5.Proofs about Graphs and Pigeonhole Principle

For any graph $G = (V, E)$, if $G$ is not connected, then $G^C$ is connected.

证明方法：对任意两点分类讨论，依据它们是否在同一**连通分量**内。（聪明的视角）

### The Pigeonhole Principle

**广义鸽巢原理 generalized pigeonhole principle**：把 $m$ 个物品放到 $n$ 个盒子，必有一个盒子里面至少有 $\lceil m/n \rceil$ 个物体；必有一个盒子里面至多有 $\lfloor m/n \rfloor$ 个物体。

**Ramsey Theorem**：对任意正整数 $s, t$ 都存在一个最小整数 $R(s, t)$ 使得：对完全图 $K_{R(s, t)}$ 的边进行红蓝二染色，必然出现红色 $K_s$ 或蓝色 $K_t$。

特例 $R(3, 3)=6$。任意 6 个人的聚会，要么有 3 个人互相认识，要么有 3 个人互相不认识。

哲学：结构够大，就必然有秩序出现。

当且仅当版本：如果把 $m$ 个物体分配到 $n$ 个盒子里，存在一个盒子装有多于 $m/n$ 个物体，当且仅当存在一个盒子装有少于 $m/n$ 个物体。（偏离平均值的情况必然成对出现）

## 8～10.Finite Automata, Regular Expression

**Computability Theory**

自动机 automaton 是对计算机的抽象。

**Computing with Finite Memory**

**有限状态自动机**

我们将把一个有限内存的计算机建模为一组由**转移**（transitions）连接的**状态**（states）的集合。每个状态对应设备内存的一种可能配置。每个转移表示内存如何响应输入而发生变化。某个状态被指定为起始状态。计算从该状态开始。

这个设备处理由字符组成的字符串。每个字符代表对设备的一次外部输入。字符串代表对设备的完整输入序列。要运行这个设备，我们从起始状态开始，从左到右扫描输入。每当机器看到一个字符，它就通过沿着标有该字符的转移来改变状态。

一旦我们输入完所有的字符，就需要得到计算的结果。一般来说，计算机可以产生各种各样的结果：一个数字、一段文字等等。作为一个简化假设，我们**假定只需要得到一个单一的比特作为输出**。也就是说，我们的机器只会说“是（YES）”或“否（NO）”。（因为任何复杂输出，都可以拆成一系列“是/否”问题。）

在我们的计算设备中，有些状态会被标记为接受状态。这些状态用双圈来表示。如果在接收状态结束，返回 YES。否则 NO。

确定性有限状态自动机（Deterministic Finite Automaton，DFA）体现在它的判定过程是确定性的。

对于一个自动机，它识别的**语言**就定义为它接受的全部子串的集合。

如果存在一个 DFA $D$ 使得 $D$ 所接受的语言 $\mathcal{L}(D)$ 恰好等于语言 $L$，则 $L$ 称为**正则语言**。称 $D$ 识别 $L$。

非确定性有限状态自动机 NFA。如果一个计算模型在计算的每一个步骤中，都有有限个（可能为零个）选择可供做出，那么这个模型就是非确定性的（nondeterministic）。只要存在任意一个选择序列能够引导机器进入接受状态，那么这台机器就接受该输入。

NFA 有 $\varepsilon$-转移，不消耗输入就能进行。

理解 NFA：

- Perfect positive guessing：它总是能神奇地猜中哪一条路最终会通向接受状态。positive 在于只要存在就判定接受。
- Massive parallelism：一个 NFA 可以被看作是同一时间可以处于多个状态的 DFA。在每一个时间点，当 NFA 需要执行转移时，它会同时尝试所有的选项。实际上是在同时维护一个“**当前可能处于的状态集合**”。

---

NFA 转换为 DFA 的时候，DFA 的状态对应 NFA 状态的集合。

定理：一个语言 $L$ 是正则语言当且仅当有 NFA $N$ 使得 $\mathcal{L}(N)=L$。

这样我们可以用两种方法对正则语言进行推理。

定理：如果 $L$ 是正则语言，那么 $\overline{L}$ 也是正则语言。

因为正则语言都可以被 DFA 识别。把一个 DFA 的所有接受状态和非接受状态翻转，就能得到识别补集的 DFA。既然结果依然能被 DFA 识别，那它自然还是正则语言。

因此说正则语言在补运算下封闭。

---

克林闭包（Kleene Closure）是对语言的一种操作。$L^*=\{w\in \Sigma ^* | \exist n \in \mathbb{N}. w \in L^{n} \}$

$L^{n}$ 就是 $L$ 的字符串通过连接零次或多次（允许重复）所能形成的所有可能字符串的集合。

**封闭性**

如果 $L_1, L_2$ 是字母表 $\Sigma$ 的语言，那么以下也是正则语言：

- $\overline{L_1}$
- $L_1 \cup L_2$
- $L_1 \cap L_2$
- $L_1 L_2$
- $L_1 ^*$

---

**Regular Expressions**

自下而上地构建正则语言：从小的简单正则语言开始，利用封闭性构造。

**正则表达式**是一种通过字符串表示来描述语言的方法。

运算优先级：$(R)$ $R^*$ $R_1 R_2$ $R_1 \cup R_2$

例如：$\Sigma = \{a, b\}$，$L = \{w\in \Sigma ^* | w \ contains\ aa \ as \ a \ substring\}$。正则表达式设计为 $(a\cup b)^*aa(a\cup b)^*$。

如果 $R$ 是一个正则表达式，那么由 $R$ 所描述的语言 $\mathcal{L}(R)$ 是正则的。

证明：原子正则表达式都表示正则语言。组合步骤对应着封闭性。因此，由它们构成的任何东西都必定是正则的！

定理：如果 $L$ 是一个正则语言，那么就存在一个对应的正则表达式。

证明思路：将任意一个 NFA 转化为正则表达式。

状态消去算法 The State-Elimination Algorithm：

- 从 $L$ 对应的 NFA $N$ 开始
- 添加一个新的初始状态 $q_s$ 和接受状态 $q_f$
  - 添加 $\epsilon$-转移：从 $q_s$ 添加一条指向 $N$ 的旧起始状态。
  - 从 $N$ 的每一个旧接受状态，添加一条指向新的接受状态 $q_f$，并将旧接受状态标记为非接受状态。
- 从 NFA 中反复移除除了 $q_s$ 和 $q_f$ 之外的其他状态，方法叫“短路（shortcutting）”，也就是将经过被消除状态的所有路径合并成直接连接的路径，并将路径上的正则表达式进行合并。直到只剩下 $q_s$ 和 $q_f$。
- 最后从 $q_s$ 到 $q_f$ 的唯一一条转移边上标注的正则表达式就是 $L$ 的等价正则表达式。

**定理**：以下陈述都是等价的：

* $L$ 是一个正则语言。
* 存在一个 DFA（确定性有限自动机）$D$，使得 $\mathcal{L}(D) = L$。
* 存在一个 NFA（非确定性有限自动机）$N$，使得 $\mathcal{L}(N) = L$。
* 存在一个正则表达式 $R$，使得 $\mathcal{L}(R) = L$。

---

正则语言对应于可以用有限内存解决的问题。在任何一个时间点，我们只需要存储有限多条信息中的一条。

从某种意义上说，非正则语言对应于无法用有限内存解决的问题。

既然人类建造的每一台计算机都只有有限的内存，那么在某种意义上，非正则语言对应的就是物理计算机无法解决的问题！

$E=\{a^n b^n | n\in \mathbb{N} \}$ 是非正则的。

区分集：假设有两个不同的字符串 $x, y$，如果存在某个后缀字符串 $z$ 使得字符串 $xz$ 属于语言 $L$ 而 $yz$ 不属于，就说后缀 $z$ 能区分 $x, y$。一个区分集 $S$ 就是指在这个集合里任意两个不同的字符串都可以被某个后缀区分开。

Myhill-Nerode 定理：如果 $L$ 是一个语言，且 $S$ 是 $L$ 的一个包含无限多个字符串的区分集，那么 $L$ 不是正则语言。

---

DFA 就是有限内存的计算机，正则语言对应的是用有限内存可以解决的问题。现在我们知道计算机能解决的问题是有限的。

## 11～12. CFGs, Turing Machines

**Context-Free Grammars**

上下文无关文法（CFG）是一套递归的规则，用来定义一种语言。

一个上下文无关文法（CFG）在形式上是一个包含四个项目的集合：

1. 非终结符集合（Nonterminal symbols / variables）
2. 终结符集合（Terminal symbols）
3. 产生式规则集合（Production rules）：说明每个非终结符如何被替换成终结符和非终结符组成的字符串。
4. 起始符号（Start symbol）

Derivation（推导）就是把 CFG 规则用一遍的过程。

If $G$ is a CFG with alphabet $\Sigma$ and start symbol $S$, then the **language of** $G$  is the set $\mathcal{L}(G) = \{ \omega \in \Sigma^* \mid S \Rightarrow^* \omega \} $

语言 $L$ 被称为上下文无关语言，当有一个 CFG $G$ 满足 $L=\mathcal{L}(G)$

CFG 的规则只有 $\to$，没有像正则表达式一样的 $*$ 特殊操作。

定理：每个正则语言都是上下文无关的。

推导过程有无限的记忆。（通过递归，建立了无限记忆的栈）

---

**Turing Machines**

TM 只有六种命令：`Move`, `Write`, `Goto`, `Return`, `If`, `If Not`

recognizer, RE

decider, R

以后再学吧～