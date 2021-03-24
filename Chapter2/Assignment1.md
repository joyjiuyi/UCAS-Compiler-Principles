# 编译原理 第 2 章 作业 1

## 练习 2.2.1
虑下面的上下文无关文法：
`S → S S + | S S * | a`
1. 试说明如何使用文法生成串 `aa+a*`
2. 试为这个串构造一颗语法分析树
3. 该文法生成的语言是什么？
4. 该文法具有二义性吗？为什么？

### 解
1. 可按照下面的方式生成串 `aa+a*`：  
   <code><u>S</u> → S <u>S</u> * → <u>S</u> a* → S <u>S</u> +a* → <u>S</u> a+a* → aa+a*</code>

2. 串 `aa+a*` 的语法分析树如下：  
   <img style="width:9.5em;" src="img1.svg">

3. 该文法生成的语言是 `L = {包含加号、乘号、数字的后缀表达式}`

4. 该文法不具有二义性。对于任意一个终结符串，其总可以从最后一个终结符确定其使用的文法，使得始终只有一种语法分析树。


## 练习 2.2.5
1. 证明：用下面文法生成的所有二进制串的值都能被 `3` 整除（提示：对语法分析树的结点数目使用数学归纳法）  
`num → 11 | 1001 | num 0 | num num`
2. 上面的文法是否能够生成所有能被 `3` 整除的二进制串？

### 解
1. 证明如下：
   
   1. 若用上述文法生成的二进制串的语法分析树只有 2 个节点，那么二进制串只可能为 `11` 或者 `1001`。显然，二进制串 `11`（即十进制 3）和 `1001`（即十进制 9）皆可被 `3` 整除。

   2. 假设节点数小于 `k`（`k>2`）的语法分析树对应的二进制串都能被 `3` 整除，考虑节点数为 `k` 的语法分析树，其根节点使用的语法可能是 `num → num num` 或者 `num → num 0`。
   
      考虑根节点为 `num → num₁ num₂`。根据假设，`num₁` 和 `num₂` 的值均可被 `3` 整除，不妨设 `num₁.v = 3 × m`，`num₂.v = 3 × n`，那么整个二进制串的值为 <code>num.v = num₁.v × 2<sup>t</sup> + num₂ = 3 × m × 2<sup>t</sup> + 3 × n = 3 ×(2<sup>t</sup> + n)</code>，显然可以被 `3` 整除。（其中 `t` 为 `num₂` 的位数）

      考虑根节点为 `num → num₁ 0`。根据假设，`num₁` 的值均可被 `3` 整除，不妨设 `num₁.v = 3 × m`，那么整个二进制串的值为 <code>num.v = num₁.v × 2 = 3 × m × 2</code>，显然可以被 `3` 整除。

   根据数学归纳法，由 1、2 易得，用上述文法生成的所有二进制串的值都能被 `3` 整除。

2. 并不能。上述文法生成的二进制串，总包含 `11` 或者 `1001` 的子串，但考虑二进制串 `10101`（即十进制 `21`），其可被 `3` 整除，但显然不可被上述文法生成。

## 练习 2.3.1
构建一个语法制导翻译方案，该方案把算术表达式从中缀表示方式翻译为运算符在运算分量之前的前缀表示方式。例如，`-xy` 是表达式 `x-y` 的前缀表示。给出输入 `9-5+2` 和 `9-5*2` 的注释分析树。

### 解
翻译方案如下：
```
expr   → {print('+')} expr + term
expr   → {print('-')} expr – term
expr   → term
term   → {print('*')} term * factor
term   → {print('/')} term / factor
term   → factor
factor → {print(digit)} digit
factor → ( expr )
```

`9-5+2` 的注释分析树：

<img style="width:21.24em;" src="img2.svg">

`9-5*2` 的注释分析树：

<img style="width:22.95em;" src="img3.svg">