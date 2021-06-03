# 编译原理 第 6 章 作业 3

## 1
在图 6-36 的语法制导定义中添加处理下列控制流构造的规则：

(1) 一个 $\mathbf{repeat}$ 语句， $\mathbf{repeat} \ S \ \mathbf{while} \ B$  
(2) 一个 $\mathbf{for}$ 循环语句， $\mathbf{for} \ ( \ S_1 \ ; \ B \ ; \ S_2 \ ) \ S_3$

### 解

规则如下：

| Production | Semantic Rules|
| :-- | :---- |
| $S \to \mathbf{repeat} \ S_1 \ \mathbf{while} \ B$<br><br><br><br> | $S_1.next = newlabel()$<br>$B.true = newlabel()$<br>$B.false = S.next$<br>$S.code = label(B.true) \parallel S_1.code \parallel label(S_1.next) \parallel B.code$|
| $S \to \mathbf{for} \ ( \ S_1 \ ; \ B \ ; \ S_2 \ ) \ S_3$<br><br><br><br><br><br><br> | $S_1.next = newlabel()$<br>$B.true = newlabel()$<br>$B.false = S.next$<br>$S_2.next = S_1.next$<br>$S_3.next = newlabel()$<br>$S.code = S_1.code \parallel label(S_1.next) \parallel B.code \parallel label(B.true)$<br>$\qquad \parallel S_3.code \parallel label(S_3.next) \parallel S_2.code \parallel gen(\text{`}\mathtt{goto}\text{'},S_1.next)$<br>

## 2
使用图 6-43 中的翻译方案翻译下列表达式
```
! ( a == b ) && ( c < d || e != f )
```
给出分析过程，给出带 $\mathbb{truelist}$ 和 $\mathbb{flaselist}$ 的注释语法分析树。
假设第一条被生成的指令的地址是 100。


### 解

分析过程：

1. 利用 5) $B \to E_1 \ \mathbf{rel} \ E_2$ 规约 `a == b`（记作 `B1`），生成代码
   ```
   100: if a == b goto -
   101: goto - 
   ```
   并得
   $$
   \begin{alignedat}{2}
    &\texttt{B1}.truelist & &= \{ 100 \}\\
    &\texttt{B1}.falselist & &= \{ 101 \}
   \end{alignedat}
   $$

2. 利用 3) $B \to \ ! \ B_1$ 和 4) $B \to ( \ B_1 )$ 规约 `! ( B1 )`（记作 `B2`），得到
   $$
   \begin{alignedat}{2}
    &\texttt{B2}.truelist & &= \{ 101 \}\\
    &\texttt{B2}.falselist & &= \{ 100 \}
   \end{alignedat}
   $$
   
3. 利用 5) $B \to E_1 \ \mathbf{rel} \ E_2$ 规约 `c < d`（记作 `B3`），生成代码
   ```
   102: if c < d goto -
   103: goto - 
   ```
   并得
   $$
   \begin{alignedat}{2}
    &\texttt{B3}.truelist & &= \{ 102 \}\\
    &\texttt{B3}.falselist & &= \{ 103 \}
   \end{alignedat}
   $$

4. 利用 5) $B \to E_1 \ \mathbf{rel} \ E_2$ 规约 `e != f`（记作 `B4`），生成代码
   ```
   104: if e != f goto -
   105: goto - 
   ```
   并得
   $$
   \begin{alignedat}{2}
    &\texttt{B4}.truelist & &= \{ 104 \}\\
    &\texttt{B4}.falselist & &= \{ 105 \}
   \end{alignedat}
   $$

5. 利用 1) $B \to B_1 \ \texttt{||} \ M \ B_2$ 和 8) $M \to \varepsilon$ 规约 `B3 || B4`（记作 `B5`）  
   首先，通过 8)，得到
   $$
   \begin{alignedat}{2}
    &M.instr & &= \{ 104 \}\\
   \end{alignedat}
   $$
   然后，通过 1)，执行
   $$backpatch(B_1.falselist, M.instr);$$
   即
   $$backpatch(\texttt{B3}.falselist = \{103\}, M.instr = 104);$$
   填充代码
   ```
   103: goto 104
   ```
   并得
   $$
   \begin{alignedat}{2}
    &\texttt{B5}.truelist & &= \{ 102, 104 \}\\
    &\texttt{B5}.falselist & &= \{ 105 \}\\
   \end{alignedat}
   $$
6. 利用 4) $B \to ( \ B_1 \ )$ 规约 `( B5 )`（记作 `B6`），得到
   $$
   \begin{alignedat}{2}
    &\texttt{B6}.truelist & &= \{ 102, 104 \}\\
    &\texttt{B6}.falselist & &= \{ 105 \}\\
   \end{alignedat}
   $$
7. 利用 2) $B \to B_1 \ \texttt{\&\&} \ M \ B_2$  和 8) $M \to \varepsilon$ 规约 `B2 && B6`（记作 `B7`）  
   首先，通过 8)，得到
   $$
   \begin{alignedat}{2}
    &M.instr & &= \{ 102 \}\\
   \end{alignedat}
   $$
   然后，通过 1)，执行
   $$backpatch(B_1.truelist, M.instr);$$
   即
   $$backpatch(\texttt{B2}.truelist = \{101\}, M.instr = 102);$$
   填充代码
   ```
   101: goto 102
   ```
   并得
   $$
   \begin{alignedat}{2}
    &\texttt{B7}.truelist & &= \{ 102, 104 \}\\
    &\texttt{B7}.falselist & &= \{ 100, 105 \}\\
   \end{alignedat}
   $$

如果用 LTrue 和 LFalse 表示表达式分别为真、假的两个出口，那么得到的三地址指令如下
```
   100: if a == b goto LFalse
   101: goto 102
   102: if c < d goto LTrue
   103: goto 104
   104: if e != f goto LTrue
   105: goto LFalse
```

语法分析树如下：

<img src="./3-2-1.svg">