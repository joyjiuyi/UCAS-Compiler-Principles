# 编译原理 第 4 章 作业 3

## 1
对于文法 $S \to S\, S\, \mathtt{+} \mid S\, \mathtt{*} \mid \mathtt{a}$ 和下面的最右句型，指出其归约时使用的到句柄

(1) $S\, S\, \mathtt{+}\, \mathtt{a}\, \mathtt{*}\, \mathtt{+}$  
(2) $S\, S\, \mathtt{*}\, \mathtt{a}\, \mathtt{+}$  
(3) $\mathtt{a}\, \mathtt{a}\, \mathtt{*}\, \mathtt{a}\, \mathtt{+}\, \mathtt{+}\, \mathtt{a}\, \mathtt{+}$

### 解
(1) 句柄是开头处的 $S \to S\, S\, \mathtt{+}$

(2) 句柄是 $S$ 后的 $S \to S\, \mathtt{*}$

(3) 句柄是开头处的 $S \to \mathtt{a}$

## 2
对于文法 $S \to S\, S\, \mathtt{+} \mid S\, \mathtt{*} \mid \mathtt{a}$，

(1) 增广该文法构造 SLR 项目集。  
(2) 计算这些项目集的 GOTO 函数，给出这个文法的语法分析表。  
(3) 这个文法是 SLR 文法吗？

### 解

增广后的文法如下：
$$
\begin{aligned}
    &(1)\quad S' \to S \\
    &(2)\quad S \to S\, S\, \mathtt{+} \\
    &(3)\quad S \to S\, \mathtt{*} \\
    &(4)\quad S \to \mathtt{a}
\end{aligned}
$$

根据该文法构造出的 LR(0) 状态机如下，其给出了 SLR 项目集和 GOTO 函数：

<img src="./3-2-LR0.svg" style="">

同时：
$$
\begin{aligned}
    \mathrm{FOLLOW}(S') &= \{\mathtt{\$}\} \\
    \mathrm{FOLLOW}(S) &= \{\mathtt{+}, \mathtt{*}, \mathtt{a}, \mathtt{\$}\} \\
\end{aligned}
$$

文法分析表如下：
<table>
<thead>
<tr>
<th style="text-align:center" rowspan="2">STATE</th>
<th style="text-align:center" colspan="4">ACTION</th>
<th style="text-align:center" colspan="1">GOTO</th>
</tr>
<tr>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi mathvariant="monospace">a</mi></mrow><annotation encoding="application/x-tex">\mathtt{a}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.43056em;vertical-align:0em;"></span><span class="mord"><span class="mord mathtt">a</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mo lspace="0em" rspace="0em">+</mo></mrow><annotation encoding="application/x-tex">\mathtt{+}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.66666em;vertical-align:-0.08333em;"></span><span class="mord"><span class="mord">+</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mo lspace="0em" rspace="0em">∗</mo></mrow><annotation encoding="application/x-tex">\mathtt{*}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.46528em;vertical-align:0em;"></span><span class="mord"><span class="mord">∗</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi mathvariant="monospace">$</mi></mrow><annotation encoding="application/x-tex">\mathtt{\$}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.77777em;vertical-align:-0.08333em;"></span><span class="mord"><span class="mord mathtt">$</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi>S</mi></mrow><annotation encoding="application/x-tex">S</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.68333em;vertical-align:0em;"></span><span class="mord mathdefault" style="margin-right:0.05764em;">S</span></span></span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">0</td>
<td style="text-align:center">s5</td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
<td style="text-align:center">1</td>
</tr>
<tr>
<td style="text-align:center">1</td>
<td style="text-align:center">s5</td>
<td style="text-align:center"></td>
<td style="text-align:center">s4</td>
<td style="text-align:center">acc</td>
<td style="text-align:center">2</td>
</tr>
<tr>
<td style="text-align:center">2</td>
<td style="text-align:center">s5</td>
<td style="text-align:center">s3</td>
<td style="text-align:center">s4</td>
<td style="text-align:center"></td>
<td style="text-align:center">2</td>
</tr>
<tr>
<td style="text-align:center">3</td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">4</td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">5</td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center"></td>
</tr>
</tbody>
</table>
| <!--  | STATE | $\mathtt{a}$ | $\mathtt{+}$ | $\mathtt{*}$ | $\mathtt{\$}$ | $S$ |
| :---: | :---: | :----------: | :----------: | :----------: | :-----------: |
|   0   |  s5   |              |              |              |       1       |
|   1   |  s5   |              |      s4      |     acc      |       2       |
|   2   |  s5   |      s3      |      s4      |              |       2       |
|   3   |  r2   |      r2      |      r2      |      r2      |               |
|   4   |  r3   |      r3      |      r3      |      r3      |               |
|   5   |  r4   |      r4      |      r4      |      r4      |               | --> |

<!-- $$
\begin{aligned}
S' &\to \cdot\, S \\
S' &\to S \cdot \\
S &\to \cdot\, S\, S\, \mathtt{+} \\
S &\to S \cdot S\, \mathtt{+} \\
S &\to S\, S \cdot \mathtt{+} \\
S &\to S\, S\, \mathtt{+}\, \cdot  \\
S &\to \cdot\, S\, \mathtt{*} \\
S &\to S \cdot \mathtt{*} \\
S &\to S\, \mathtt{*}\, \cdot \\
S &\to \cdot\, \mathtt{a} \\
S &\to \mathtt{a}\, \cdot \\
\end{aligned}
$$

$$
\begin{aligned}
S' &\to \cdot\, S \\
S &\to \cdot\, S\, S\, \mathtt{+} \\
S &\to \cdot\, S\, \mathtt{*} \\
S &\to \cdot\, \mathtt{a} \\
\end{aligned}
$$
$$
\begin{aligned}
S' &\to S\, \cdot \\
S &\to S \cdot S\, \mathtt{+} \\
S &\to S \cdot \mathtt{*} \\
S &\to \cdot\, S\, S\, \mathtt{+} \\
S &\to \cdot\, S\, \mathtt{*} \\
S &\to \cdot\, \mathtt{a} \\
\end{aligned}
$$
$$
\begin{aligned}
S &\to S\, S \cdot \mathtt{+} \\
S &\to S \cdot S\, \mathtt{+} \\
S &\to S \cdot \mathtt{*} \\
S &\to \cdot\, S\, S\, \mathtt{+} \\
S &\to \cdot\, S\, \mathtt{*} \\
S &\to \cdot\, \mathtt{a} \\
\end{aligned}
$$
$$
\begin{aligned}
\end{aligned}
$$
$$
\begin{aligned}
\end{aligned}
$$ -->
<br>
分析表中没有冲突，故该文法是 SLR 文法。

<div style="page-break-after: always;"></div>

## 3
对于文法 $S \to S\, S\, \mathtt{+} \mid S\, \mathtt{*} \mid \mathtt{a}$，

(1) 构造规范 LR 项目集族。  
(2) 构建其语法分析表。  
(3) 构建 LALR 项目集族。

### 解

增广后的文法如下：
$$
\begin{aligned}
    &(1)\quad S' \to S \\
    &(2)\quad S \to S\, S\, \mathtt{+} \\
    &(3)\quad S \to S\, \mathtt{*} \\
    &(4)\quad S \to \mathtt{a}
\end{aligned}
$$


同时：
$$
\begin{aligned}
    \mathrm{FIRST}(S') &= \{\mathtt{a}\} \\
    \mathrm{FIRST}(S) &= \{\mathtt{a}\} \\
\end{aligned}
$$

可以构造出如下的 GOTO 图，其给出了规范 LR 项目集族：

<img src="./3-3-LR1.svg" style="">

<div style="page-break-after: always;"></div>

其语法分析表如下：<table>
<thead>
<tr>
<th style="text-align:center" rowspan="2">STATE</th>
<th style="text-align:center" colspan="4">ACTION</th>
<th style="text-align:center" colspan="1">GOTO</th>
</tr>
<tr>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi mathvariant="monospace">a</mi></mrow><annotation encoding="application/x-tex">\mathtt{a}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.43056em;vertical-align:0em;"></span><span class="mord"><span class="mord mathtt">a</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mo lspace="0em" rspace="0em">+</mo></mrow><annotation encoding="application/x-tex">\mathtt{+}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.66666em;vertical-align:-0.08333em;"></span><span class="mord"><span class="mord">+</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mo lspace="0em" rspace="0em">∗</mo></mrow><annotation encoding="application/x-tex">\mathtt{*}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.46528em;vertical-align:0em;"></span><span class="mord"><span class="mord">∗</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi mathvariant="monospace">$</mi></mrow><annotation encoding="application/x-tex">\mathtt{\$}</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.77777em;vertical-align:-0.08333em;"></span><span class="mord"><span class="mord mathtt">$</span></span></span></span></span></th>
<th style="text-align:center"><span class="katex"><span class="katex-mathml"><math xmlns="http://www.w3.org/1998/Math/MathML"><semantics><mrow><mi>S</mi></mrow><annotation encoding="application/x-tex">S</annotation></semantics></math></span><span class="katex-html" aria-hidden="true"><span class="base"><span class="strut" style="height:0.68333em;vertical-align:0em;"></span><span class="mord mathdefault" style="margin-right:0.05764em;">S</span></span></span></span></th>
</tr>
</thead>
<tbody>
<tr>
<td style="text-align:center">0</td>
<td style="text-align:center">s4</td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
<td style="text-align:center">1</td>
</tr>
<tr>
<td style="text-align:center">1</td>
<td style="text-align:center">s7</td>
<td style="text-align:center"></td>
<td style="text-align:center">s5</td>
<td style="text-align:center">acc</td>
<td style="text-align:center">2</td>
</tr>
<tr>
<td style="text-align:center">2</td>
<td style="text-align:center">s7</td>
<td style="text-align:center">s6</td>
<td style="text-align:center">s8</td>
<td style="text-align:center"></td>
<td style="text-align:center">3</td>
</tr>
<tr>
<td style="text-align:center">3</td>
<td style="text-align:center">s7</td>
<td style="text-align:center">s9</td>
<td style="text-align:center">s8</td>
<td style="text-align:center"></td>
<td style="text-align:center">3</td>
</tr>
<tr>
<td style="text-align:center">4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center"></td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">5</td>
<td style="text-align:center">r3</td>
<td style="text-align:center"></td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">6</td>
<td style="text-align:center">r2</td>
<td style="text-align:center"></td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">7</td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center">r4</td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">8</td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center">r3</td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
</tr>
<tr>
<td style="text-align:center">9</td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center">r2</td>
<td style="text-align:center"></td>
<td style="text-align:center"></td>
</tr>
</tbody>
</table>
<!-- | STATE | $\mathtt{a}$ | $\mathtt{+}$ | $\mathtt{*}$ | $\mathtt{\$}$ |  $S$  |
| :---: | :----------: | :----------: | :----------: | :-----------: | :---: |
|   0   |      s4      |              |              |               |   1   |
|   1   |      s7      |              |      s5      |      acc      |   2   |
|   2   |      s7      |      s6      |      s8      |               |   3   |
|   3   |      s7      |      s9      |      s8      |               |   3   |
|   4   |      r4      |              |      r4      |      r4       |       |
|   5   |      r3      |              |      r3      |      r3       |       |
|   6   |      r2      |              |      r2      |      r2       |       |
|   7   |      r4      |      r4      |      r4      |               |       |
|   8   |      r3      |      r3      |      r3      |               |       |
|   9   |      r2      |      r2      |      r2      |               |       | -->

<!-- $$
\begin{aligned}
S' &\to \cdot\, S , \mathtt{\$}\\
S &\to \cdot\, S\, S\, \mathtt{+}, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, \mathtt{*} , \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, \mathtt{a} , \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$

$$
\begin{aligned}
S' &\to S\, \cdot , \mathtt{\$}\\
S &\to S \cdot S\, \mathtt{+}, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
S &\to S \cdot \mathtt{*} , \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, S\, \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, \mathtt{*} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, \mathtt{a} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S &\to S\, S \cdot \mathtt{+}, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
S &\to S \cdot S\, \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to S \cdot \mathtt{*} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, S\, \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, \mathtt{*} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, \mathtt{a} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S &\to S\, S \cdot \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to S \cdot S\, \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to S \cdot \mathtt{*} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, S\, \mathtt{+}, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, S\, \mathtt{*} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
S &\to \cdot\, \mathtt{a} , \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to \mathtt{a}\, \cdot, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to \mathtt{a}\, \cdot, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to S\, \mathtt{*}\, \cdot, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to S\, \mathtt{*}\, \cdot, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to S\, S\, \mathtt{+}\, \cdot, \mathtt{\$}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$
$$
\begin{aligned}
S \to S\, S\, \mathtt{+}\, \cdot, \mathtt{+}/\mathtt{a}/\mathtt{*} \\
\end{aligned}
$$ -->

归并出如下的 LALR GOTO 图，其给出了 LALR 项目集族：

<img src="./3-3-LALR.svg" style="">