# 编译原理 第 4 章 作业 2

## 1
使得文法的预测分析产生回溯的原因是什么？

仅使用 FIRST 集合可以避免回溯吗？为什么？

### 解
使用文法的预测分析时，当非终结符用某个选择匹配成功时，这种成功可能是暂时的。这种虚假现象的存在，导致了回溯的产生。

仅使用 FIRST 集合不能避免回溯。

考虑产生式 $A \to \alpha \mid \beta$，设 $\mathrm{FIRST}(\alpha) \cap \mathrm{FIRST}(\beta) = \emptyset$，但 $\varepsilon$ 是 $\mathrm{FIRST}(\alpha)$ 或 $\mathrm{FIRST}(\beta)$ 中的元素。

不失一般性，我们设 $\varepsilon \in \mathrm{FIRST}(\alpha)$。在这种情况下，我们就必须得根据 $\mathrm{FOLLOW}(\alpha)$ 和 $\mathrm{FIRST}(\beta)$ 的情况来判断如何选择产生式。


## 2
考虑文法：
$$
\begin{aligned}
   \mathit{rexpr} &\to \mathit{rexpr} \;\mathtt{+}\; \mathit{rterm} \mid \mathit{rterm} \\
   \mathit{rterm} &\to \mathit{rterm} \; \mathit{rfactor} \mid \mathit{rfactor} \\
   \mathit{rfactor} &\to \mathit{rfactor} \; \mathtt{*} \mid \mathit{rprimary} \\
   \mathit{rprimary} &\to \mathtt{a} \mid \mathtt{b} \\
\end{aligned}
$$

1) 消除左递归
2) 求得该文法的 FIRST 集合和 FOLLOW 集合
3) 说明所得的文法是 LL(1) 文法
4) 为所得的文法构造 LL(1) 分析表
5) 对输入串 $\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}$ 给出相应的 LL(1) 分析程序的动作

### 解

(1) 消除左递归后得到的文法如下：
   $$
   \begin{aligned}
      \mathit{rexpr} &\to \mathit{rterm} \; \mathit{rexpr'} \\
      \mathit{rexpr'} &\to \mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'} \mid \varepsilon \\
      \mathit{rterm} &\to \mathit{rfactor} \; \mathit{rterm'} \\
      \mathit{rterm'} &\to \mathit{rfactor} \; \mathit{rterm'} \mid \varepsilon \\
      \mathit{rfactor} &\to \mathit{rprimary} \; \mathit{rfactor'} \\
      \mathit{rfactor'} &\to \mathtt{*} \; \mathit{rfactor'} \mid \varepsilon \\
      \mathit{rprimary} &\to \mathtt{a} \mid \mathtt{b} \\
   \end{aligned}
   $$

(2) 该文法的 FIRST 集合为：
   $$
   \begin{aligned}
      \mathrm{FIRST}(\mathit{rexpr}) &= \{\mathtt{a}, \mathtt{b}\} \\
      \mathrm{FIRST}(\mathit{rexpr'}) &= \{\mathtt{+}, \varepsilon\} \\
      \mathrm{FIRST}(\mathit{rterm}) &= \{\mathtt{a}, \mathtt{b}\} \\
      \mathrm{FIRST}(\mathit{rterm'}) &= \{\mathtt{a}, \mathtt{b}, \varepsilon\} \\
      \mathrm{FIRST}(\mathit{rfactor}) &= \{\mathtt{\mathtt{a}, \mathtt{b}}\} \\
      \mathrm{FIRST}(\mathit{rfactor'}) &= \{\mathtt{*}, \varepsilon\} \\
      \mathrm{FIRST}(\mathit{rprimary}) &= \{\mathtt{\mathtt{a}, \mathtt{b}}\} \\
   \end{aligned}
   $$

   该文法的 FOLLOW 集合为：
   $$
   \begin{aligned}
      \mathrm{FOLLOW}(\mathit{rexpr}) &= \{ \mathtt{\$} \} \\
      \mathrm{FOLLOW}(\mathit{rexpr'}) &= \{ \mathtt{\$} \} \\
      \mathrm{FOLLOW}(\mathit{rterm}) &= \{ \mathtt{\$}, \mathtt{+} \} \\
      \mathrm{FOLLOW}(\mathit{rterm'}) &= \{ \mathtt{\$}, \mathtt{+} \} \\
      \mathrm{FOLLOW}(\mathit{rfactor}) &= \{ \mathtt{\$}, \mathtt{+}, \mathtt{a}, \mathtt{b} \} \\
      \mathrm{FOLLOW}(\mathit{rfactor'}) &= \{ \mathtt{\$}, \mathtt{+}, \mathtt{a}, \mathtt{b} \} \\
      \mathrm{FOLLOW}(\mathit{rprimary}) &= \{ \mathtt{\$}, \mathtt{+}, \mathtt{*}, \mathtt{a}, \mathtt{b} \} \\
   \end{aligned}
   $$

(3) 说明所得的文法是 LL(1) 文法：
   
   * 对以 $\mathit{rexpr'}$ 为左部的生成式：  
     
     $\mathrm{FIRST}(\mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'}) \cap \mathrm{FIRST}(\varepsilon) = \mathrm{FIRST}(\mathtt{+}) \cap \mathrm{FIRST}(\varepsilon) = \emptyset$，  
     且 $\mathrm{FIRST}(\mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'}) \cap \mathrm{FOLLOW}(\mathit{rexpr'}) = \mathrm{FIRST}(\mathtt{+}) \cap \mathrm{FOLLOW}(\mathit{rexpr'}) = \emptyset$

   * 对以 $\mathit{rterm'}$ 为左部的生成式：  
     
     $\mathrm{FIRST}(\mathit{rfactor} \; \mathit{rterm'}) \cap \mathrm{FIRST}(\varepsilon) = \mathrm{FIRST}(\mathit{rfactor}) \cap \mathrm{FIRST}(\varepsilon) = \emptyset$，  
     且 $\mathrm{FIRST}(\mathit{rfactor} \; \mathit{rterm'}) \cap \mathrm{FOLLOW}(\mathit{rterm'}) = \mathrm{FIRST}(\mathit{rfactor}) \cap \mathrm{FOLLOW}(\mathit{rterm'}) = \emptyset$
     
   * 对以 $\mathit{rfactor'}$ 为左部的生成式：  
     
     $\mathrm{FIRST}(\mathtt{*} \; \mathit{rfactor'}) \cap \mathrm{FIRST}(\varepsilon) = \mathrm{FIRST}(\mathtt{*}) \cap \mathrm{FIRST}(\varepsilon) = \emptyset$，  
     且 $\mathrm{FIRST}(\mathtt{*} \; \mathit{rfactor'}) \cap \mathrm{FOLLOW}(\mathit{rfactor'}) = \mathrm{FIRST}(\mathtt{*}) \cap \mathrm{FOLLOW}(\mathit{rfactor'}) = \emptyset$

   * 对以 $\mathit{rprimary}$ 为左部的生成式：  
     
     $\mathrm{FIRST}(\mathtt{a}) \cap \mathrm{FIRST}(\mathtt{a}) = \emptyset$

   * 对其他生成式左部，均只有一个对应的生成式。

   因此所得文法是 LL(1) 文法。

(4) 所得的文法的 LL(1) 分析表如下：
   
|      非终结符       | <br>$\mathtt{a}$ | <br>$\mathtt{b}$ | 输入符号<br>$\mathtt{+}$ | <br>$\mathtt{*}$ | <br>$\mathtt{\$}$ |
| :-----------------: | :----------: | :----------: | :----------: | :----------: | :-----------: |
|  $\mathit{rexpr}$   |$\mathit{rexpr} \to\\ \mathit{rterm} \; \mathit{rexpr'}$|$\mathit{rexpr} \to\\ \mathit{rterm} \; \mathit{rexpr'}$||||
|  $\mathit{rexpr'}$  |||$\mathit{rexpr'} \to\\ \mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'}$||$\mathit{rexpr'} \to \varepsilon$|
|  $\mathit{rterm}$   |$\mathit{rterm} \to\\ \mathit{rfactor} \; \mathit{rterm'}$|$\mathit{rterm} \to\\ \mathit{rfactor} \; \mathit{rterm'}$||||
|  $\mathit{rterm'}$  |$\mathit{rterm'} \to\\ \mathit{rfactor} \; \mathit{rterm'}$|$\mathit{rterm'} \to\\ \mathit{rfactor} \; \mathit{rterm'}$|$\mathit{rterm'} \to \varepsilon$||$\mathit{rterm'} \to \varepsilon$|
| $\mathit{rfactor}$  |$\mathit{rfactor} \to\\ \mathit{rprimary} \; \mathit{rfactor'}$|$\mathit{rfactor} \to\\ \mathit{rprimary} \; \mathit{rfactor'}$||||
| $\mathit{rfactor'}$ |$\mathit{rfactor'} \to \varepsilon$|$\mathit{rfactor'} \to \varepsilon$|$\mathit{rfactor'} \to \varepsilon$|$\mathit{rfactor'} \to \\ \mathtt{*} \; \mathit{rfactor'}$|$\mathit{rfactor'} \to \varepsilon$|
| $\mathit{rprimary}$ |$\mathit{rprimary} \to \mathtt{a}$|$\mathit{rprimary} \to \mathtt{b}$||||

<br>

(5) 输入串 $\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}$ 相应的 LL(1) 分析程序的动作如下：

|栈|输入|动作|
|-:|:-|:-|
|$\mathit{rexpr} \; \mathtt{\$}$|$\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rexpr} \to \mathit{rterm} \; \mathit{rexpr'}$|
|$\mathit{rterm} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rterm} \to \mathit{rfactor} \; \mathit{rterm'}$|
|$\mathit{rfactor} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor} \to \mathit{rprimary} \; \mathit{rfactor'}$|
|$\mathit{rprimary} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rprimary} \to \mathtt{a}$|
|$\mathtt{a} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor'} \to \varepsilon$|
|$\mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rterm'} \to \varepsilon$|
|$\mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rexpr'} \to \mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'}$|
|$\mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rterm} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rterm} \to \mathit{rfactor} \; \mathit{rterm'}$|
|$\mathit{rfactor} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor} \to \mathit{rprimary} \; \mathit{rfactor'}$|
|$\mathit{rprimary} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rprimary} \to \mathtt{a}$|
|$\mathtt{a} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{a} \mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor'} \to \mathtt{*} \; \mathit{rfactor'}$|
|$\mathtt{*} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{*} \mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor'} \to \varepsilon$|
|$\mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rterm'} \to \varepsilon$|
|$\mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rexpr'} \to \mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'}$|
|$\mathtt{+}\; \mathit{rterm} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{+} \mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rterm} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rterm} \to \mathit{rfactor} \; \mathit{rterm'}$|
|$\mathit{rfactor} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rfactor} \to \mathit{rprimary} \; \mathit{rfactor'}$|
|$\mathit{rprimary} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{b} \mathtt{*}\mathtt{\$}$|$\mathit{rprimary} \to \mathtt{b}$|
|$\mathtt{b} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{b} \mathtt{*}\mathtt{\$}$|match|
|$\mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{*}\mathtt{\$}$|$\mathit{rfactor'} \to \mathtt{*} \; \mathit{rfactor'}$|
|$\mathtt{*} \; \mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{*}\mathtt{\$}$|match|
|$\mathit{rfactor'} \; \mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{\$}$|$\mathit{rfactor'} \to \varepsilon$|
|$\mathit{rterm'} \; \mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{\$}$|$\mathit{rterm'} \to \varepsilon$|
|$\mathit{rexpr'} \; \mathtt{\$}$|$\mathtt{\$}$|$\mathit{rexpr'} \to \varepsilon$|
|$\mathtt{\$}$|$\mathtt{\$}$|Accept|