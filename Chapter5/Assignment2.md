# 编译原理 第 5 章 作业 2

## 1

下面是涉及运算符 $+$ 和整数（integer）或浮点（float）运算分量的表达式的文法。区分浮点数的方法是看它有无小数点。

$$
\begin{aligned}
    E & \to E \ + \ T \mid T \\
    T & \to \mathbf{num} \ . \ \mathbf{num} \mid \mathbf{num}
\end{aligned}
$$

(1) 给出一个 SDD 来确定每个项 T 和表达式 E 的类型，要求整数与浮点数运算得到浮点数。  
(2) 扩展（1）中得到的 SDD，使得它可以把表达式转换为前缀表达式。（使用一个单目运算符 “$\mathbf{intToFloat}$” 把一个整数转换为相等的浮点数，使用 “$||$” 拼接操作符和操作数）

### 解

(1) 符合要求的 SDD 如下：

| 产生式                                     | 语法规则                             |
| :----------------------------------------- | :----------------------------------- |
| 1) $E \to E_1 \ + \ T$                     | $E.type = (E_1.type == \mathrm{float} \mathrel{\mathrm{or}} T.type == \mathrm{float}) \mathrel{?} \mathrm{float} \mathrel{:} \mathrm{integer}$ |
| 2) $E \to T$                               | $E.type = T.type$                    |
| 3) $T \to \mathbf{num} \ . \ \mathbf{num}$ | $T.type = \mathrm{float}$            |
| 3) $T \to \mathbf{num}$                    | $T.type = \mathrm{integer}$          |

(2) 扩展后的 SDD 如下：

| 产生式                                     | 语法规则                             |
| :----------------------------------------- | :----------------------------------- |
| 1) $E \to E_1 \ + \ T$                     | $E.type = (E_1.type == \mathrm{float} \mathrel{\mathrm{or}} T.type == \mathrm{float}) \mathrel{?} \mathrm{float} \mathrel{:} \mathrm{integer}$<br>$\begin{aligned}E.pn ={} &(E_1.type == \mathrm{float} \mathrel{\mathrm{and}} T.type == \mathrm{integer}) \mathrel{?} (\mathbf{float+} \parallel E_1.pn \parallel \mathbf{intToFloat} \parallel T.pn ) : \\ & (E_1.type == \mathrm{integer} \mathrel{\mathrm{and}} T.type == \mathrm{float}) \mathrel{?} (\mathbf{float+} \parallel \mathbf{intToFloat} \parallel E_1.pn \parallel T.pn ) : \\ &(E_1.type == \mathrm{float} \mathrel{\mathrm{and}} T.type == \mathrm{float}) \mathrel{?} (\mathbf{float+} \parallel E_1.pn \parallel T.pn ) : \\ & (\mathbf{int+} \parallel E_1.pn \parallel T.pn) \end{aligned}$ |
| 2) $E \to T$                               | $E.type = T.type$<br>$E.pn = T.pn$                    |
| 3) $T \to \mathbf{num} \ . \ \mathbf{num}$ | $T.type = \mathrm{float}$<br>$T.pn = \mathbf{num} \ . \ \mathbf{num}$            |
| 3) $T \to \mathbf{num}$                    | $T.type = \mathrm{integer}$<br>$T.pn = \mathbf{num}$          |

$pn$ 参数为前缀表达式。

## 2
改写下面的 SDT：
$$
\begin{aligned}
A &\to A \ \{ a \} \ B \mid A \ B \ \{ b \} \mid 0\\
B &\to B \ \{ c \} \ A \mid B \ A \ \{ d \} \mid 1
\end{aligned}
$$
使得基础文法变成非左递归的。其中 $a$、$b$、$c$ 和 $d$ 是语义动作，不涉及属性计算，$0$ 和 $1$ 是终结符号。

### 解
改写后如下：

$$
\begin{aligned}
A  &\to 0 \ A'\\
A' &\to \{ a \} \ B \ A' \mid B \ \{ b \} \ A' \mid \varepsilon\\
B  &\to 1 \ B'\\
B' &\to \{ c \} \ A \ B' \mid A \ \{ d \} \ B' \mid \varepsilon
\end{aligned}
$$

## 3
（1）修改图 5-25 中的 SDD，使它包含一个综合属性 $B.le$，即一个盒子（Box）的长度。两个盒子并列及下标后得到的盒子的长度是这两个盒子的长度和。  
（2）然后，将你的新规则加入到图 5-26 中 SDT 的合适位置上


<!-- 
| Production                                         | Semantic Rules                                                                                                                                          |
| :------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1) $S \to B$                                       | $B.ps = 10$                                                                                                                                             |
| 2) $B \to B_1 \ B_2$<br><br><br><br>               | $B_1.ps = B.ps$<br>$B_2.ps = B.ps$<br>$B.ht = \max (B_1.ht, B_2.ht)$<br>$B.dp = \max (B_1.dp, B_2.dp)$                                                  |
| 3) $B \to B_1 \ \mathbf{sub}\ B_2$<br><br><br><br> | $B_1.ps = B.ps$<br>$B_2.ps = 0.7 \times B.ps$<br>$B.ht = \max (B_1.ht, B_2.ht - 0.25 \times B.ps)$<br>$B.dp = \max (B_1.dp, B_2.dp + 0.25 \times B.ps)$ |
| 4) $B \to ( \ B_1 \ )$<br><br><br>                 | $B_1.ps = B.ps$<br>$B.ht = B_1.ht$<br>$B.dp = B_1.dp$                                                                                                   |
| 5) $B \to \mathbf{text}$<br><br>                   | $B.ht = getHt(B.ps, \mathbf{text}.lexval)$<br>$B.dp = getDp(B.ps, \mathbf{text}.lexval)$                                                                |


$$
\begin{alignedat}{5}
&1)\quad&   S \ &\to&   &                   &       &\{ &   B.ps    &= 10; \}                   \\
&       &       &   & \ &B                  & \qquad&   &           &                           \\
&2)     &   B   &\to&   &                   &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1                &       &\{ &   B_2.ps  &= B.ps; \}                 \\
&       &       &   &   &B_2                &       &\{ &   B.ht    &= \max (B_1.ht, B_2.ht);   \\
&       &       &   &   &                   &       &   &   B.dp    &= \max (B_1.dp, B_2.dp); \}\\
&3)     &   B   &\to&   &                   &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1 \ \mathbf{sub} &       &\{ &   B_2.ps  &= 0.7 \times B.ps; \}      \\
&       &       &   &   &B_2                &       &\{ &   B.ht    &= \max (B_1.ht, B_2.ht - 0.25 \times B.ps);   \\
&       &       &   &   &                   &       &   &   B.dp    &= \max (B_1.dp, B_2.dp + 0.25 \times B.ps); \}\\
&4)     &   B   &\to&   &(                  &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1)               &       &\{ &   B.ht    &= B_1.ht;                  \\
&       &       &   &   &                   &       &   &   B.dp    &= B_1.dp; \}               \\
&4)     &   B   &\to&   &\mathbf{text}      &       &\{ &   B.ht    &= getHt(B.ps, \mathbf{text}.lexval);\\
&       &       &   &   &                   &       &   &   B.dp    &= getDp(B.ps, \mathbf{text}.lexval);\}\\
\end{alignedat}
$$ -->



### 解
新增的部分前添加了 $*$，以方便对比。

(1) 新的 SDD 如下：

| Production                                             | Semantic Rules                                                                                                                                          |
| :----------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 1) $S \to B$                                           | $B.ps = 10$                                                                                                                                             |
| 2) $B \to B_1 \ B_2$<br><br><br><br><br>               | $B_1.ps = B.ps$<br>$B_2.ps = B.ps$<br>* $B.le = B_1.le + B_2.le$<br>$B.ht = \max (B_1.ht, B_2.ht)$<br>$B.dp = \max (B_1.dp, B_2.dp)$                                                  |
| 3) $B \to B_1 \ \mathbf{sub}\ B_2$<br><br><br><br><br> | $B_1.ps = B.ps$<br>$B_2.ps = 0.7 \times B.ps$<br>* $B.le = B_1.le + B_2.le$<br>$B.ht = \max (B_1.ht, B_2.ht - 0.25 \times B.ps)$<br>$B.dp = \max (B_1.dp, B_2.dp + 0.25 \times B.ps)$ |
| 4) $B \to ( \ B_1 \ )$<br><br><br><br>                 | $B_1.ps = B.ps$<br>* $B.le = B_1.le$<br>$B.ht = B_1.ht$<br>$B.dp = B_1.dp$                                                                                                   |
| 5) $B \to \mathbf{text}$<br><br><br>                   | * $B.le = getLe(B.ps, \mathbf{text}.lexval)$<br>$B.ht = getHt(B.ps, \mathbf{text}.lexval)$<br>$B.dp = getDp(B.ps, \mathbf{text}.lexval)$                                                                |


(2) 新的 SDT 如下：
$$
\begin{alignedat}{5}
&1)\quad&   S \ &\to&   &                   &       &\{ &   B.ps    &= 10; \}                   \\
&       &       &   & \ &B                  & \qquad&   &           &                           \\
&2)     &   B   &\to&   &                   &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1                &       &\{ &   B_2.ps  &= B.ps; \}                 \\
&       &       &   &   &B_2                &       &\{*&   B.le    &= B_1.le + B_2.le;   \\
&       &       &   &   &                   &       &   &   B.ht    &= \max (B_1.ht, B_2.ht);   \\
&       &       &   &   &                   &       &   &   B.dp    &= \max (B_1.dp, B_2.dp); \}\\
&3)     &   B   &\to&   &                   &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1 \ \mathbf{sub} &       &\{ &   B_2.ps  &= 0.7 \times B.ps; \}      \\
&       &       &   &   &B_2                &       &\{*&   B.le    &= B_1.le + B_2.le;   \\
&       &       &   &   &                   &       &   &   B.ht    &= \max (B_1.ht, B_2.ht - 0.25 \times B.ps);   \\
&       &       &   &   &                   &       &   &   B.dp    &= \max (B_1.dp, B_2.dp + 0.25 \times B.ps); \}\\
&4)     &   B   &\to&   &(                  &       &\{ &   B_1.ps  &= B.ps; \}                 \\
&       &       &   &   &B_1)               &       &\{*&   B.le    &= B_1.le;                  \\
&       &       &   &   &                   &       &   &   B.ht    &= B_1.ht;                  \\
&       &       &   &   &                   &       &   &   B.dp    &= B_1.dp; \}               \\
&4)     &   B   &\to&   &\mathbf{text}      &       &\{*&   B.le    &= getLe(B.ps, \mathbf{text}.lexval);\\
&       &       &   &   &                   &       &   &   B.ht    &= getHt(B.ps, \mathbf{text}.lexval);\\
&       &       &   &   &                   &       &   &   B.dp    &= getDp(B.ps, \mathbf{text}.lexval);\}\\
\end{alignedat}
$$

