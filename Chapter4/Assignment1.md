# 编译原理 第 4 章 作业 1

## 练习 4.2.2
考虑上下文无关文法：  
$S \to + S S \mid * S S \mid a$

以及串 $+*aaa$

1) 给出这个串的一个最左推导；
2) 给出这个串的一个最右推导；
3) 给出这个串的一颗语法分析树；
4) 这个文法是否是二义性的？ 证明你的回答（提示：可以对串的长度做归纳) ；
5) 这个文法生成的语言是什么？

### 解

1). 最左推导：  
$S \underset{lm}\Rightarrow + S S \underset{lm}\Rightarrow + * S S S \underset{lm}\Rightarrow + * a S S \underset{lm}\Rightarrow + * a a S \underset{lm}\Rightarrow + * a a a$

2). 最右推导：  
$S \underset{rm}\Rightarrow + S S \underset{rm}\Rightarrow + S a \underset{rm}\Rightarrow + * S S a \underset{rm}\Rightarrow + * S a a  \underset{rm}\Rightarrow + * a a a$

3). 语法分析树如下：

<img style="width: 6.6em;" src="img1.svg">

4). 这个文法不具有二义性，证明如下：

首先证明一个结论：对任意一个由上述文法生成的串 $\omega$，在其右侧添加或删除任意字符，其都不再是这个文法生成的串。对长度为 $n$ 的由上述文法生成的串做归纳：

1. 考虑 $n = 1$ 时，$\omega$ 只可能为单个字符 $a$。在其右侧添加或删除字符后形成的串，显然不能由该文法生成；
   
2. 假设上述结论对 $n < k (k > 1)$ 都成立，那么考虑长度为 $n = k$ 的串 $\omega$。此时，第一步推导使用的生成式一定是 $S \Rightarrow +S_1S_2$ 或者 $S \Rightarrow *S_1S_2$。那么，此时在 $\omega$ 右侧添加或删除的任意字符，最终会转移到 $S_1$ 或 $S_2$ 上。根据归纳假设，此时 $S_1$ 或 $S_2$ 总有一个不再是由该文法生成的串，使得整个修改后的 $\omega$ 不再是该文法的串。

根据归纳，对长度为 $n$ 的由上述文法生成的串，在其右侧添加或删除任意字符，生成的新串都不再是这个文法生成的串。

然后再证明这个文法不具有二义性。对长度为 $n$ 的串做归纳：

1. 考虑 $n = 1$ 时，只有 $S \Rightarrow a$ 一种情况，显然不具有二义性；

2. 假设上述结论对 $n < k (k > 1)$ 都成立，那么考虑长度为 $n = k$ 的串 $\omega$，满足 $S\overset{*}\Rightarrow \omega$。根据 $\omega$ 开头的运算符，可以确定其第一部推导使用的生成式，不妨设为 $S \Rightarrow +S_1S_2$。
   
   从前向后对字符串进行处理。去除第一个运算符，在剩下的部分（长度为 $n - 1$）中，从左往右总能找到一个由 $S$ 推导产生的串 $\alpha$，长度设为 $m_1$。根据上面的结论，我们可以指出这个串是唯一的，不可能存在比它更长或更短的串。那么，为了满足 $S \Rightarrow +S_1S_2$ 的产生式，剩下的部分 $\beta$（长度为 $m_2 = n - 1 - m_1$）必然也是该文法产生的串。

   根据归纳假设，$\alpha$ 和 $\beta$ 都不具有二义性，均有唯一的最左推导。那么此时，串 $\omega$ 有唯一的最左推导：

   $S \underset{lm}\Rightarrow +SS \mathrel{\underset{lm}{\overset{*}\Rightarrow}} +\alpha S \mathrel{\underset{lm}{\overset{*}\Rightarrow}} + \alpha \beta$

   于是 $\omega$ 不具有二义性。

综上，上述文法不具有二义性。


5). 这个文法生成的是，包含字符 $a$、运算符 $+$、$*$ 的前缀表达式。

## 练习 4.2.3
为下面语言设计文法：所有由 0 和 1 组成的回文（palindrome) 的集合，也就是从前面和从后面读结果都相同的串的集合。

### 解
容易设计出如下的文法：  
$S \to 0\ S\ 0 \mid 1\ S\ 1 \mid 0 \mid 1 \mid \varepsilon$

## 练习 4.3.2
下面的布尔表达式对应的文法：

```
bexpr   → bexpr or bterm | bterm
bterm   → bterm and bfactor | bfactor
bfactor → not bfactor | ( bexpr ) | true | false
```

1) 对该文法提取左公因子；
2) 提取左公因子的变换能使该文法适用于自顶向下的语法分析技术吗？原因？
3) 将提取了左公因子的文法继续消除左递归；
4) 此时得到的文法适用于自顶向下的语法分析吗？

### 解

1). 该文法无左公因子。

2). 不适用于自顶向下的语法分析技术。因为存在左递归。

3). 消除左递归得到：
```
bexpr   → bterm bexpr'
bexpr'  → or bterm bexpr' | ε
bterm   → bfactor bterm'
bterm'  → and bfactor bterm' | ε
bfactor → not bfactor | ( bexpr ) | true | false
```

4). 此时得到的文法适用于自顶向下的语法分析。