# 编译原理 第 3 章 作业 1

## 练习 3.1.1
根据 3.1.2 节中的讨论，将下面的 C++ 程序
```cpp
float limitedSquare(x) { float x; 
	/* returns x-squared, but never more than 100 */
	return (x <= -10.0 || x >= 10.0) ? 100 : x * x;
}
```
划分成正确的词素序列。那些词素应该有相关联的词法值？应该具有什么值？

### 解
词素序列如下：
```
<float>
<id, pointer to symbol-table entry for "limitedSquare">
<(>
<id, pointer to symbol-table entry for "x">
<)>
<{>
<float>
<id, pointer to symbol-table entry for "x">
<return>
<(>
<id, pointer to symbol-table entry for "x">
<op, "<=">
<number, -10.0>
<op, "||">
<id, pointer to symbol-table entry for "x">
<op, ">=">
<number, 10.0>
<)>
<op, "?">
<number, 100>
<op, ":">
<id, pointer to symbol-table entry for "x">
<op, "*">
<id, pointer to symbol-table entry for "x">
<}>
```

## 练习 3.3.2
试描述下列正则表达式定义的语言（仅 1、2、5）

1). `a(a|b)*a`  
2). `((ε|a)b*)*`  
5). `(aa|bb)*((ab|ba)(aa|bb)*(ab|ba)(aa|bb)*)*`

### 解

1). 以 `a` 开头和结尾，中间有任意数量 `a` 或 `b` 的字符串。
2). 任意由 `a` 和 `b` 构成的的字符串。
5). 由偶数个 `a` 和 偶数个 `b` 构成的字符串。


## 练习 3.3.5
试写出下列语言的正则定义（仅1、8、9）

1). 包含 5 个元音的所有小写字母串，这些串中的元音按顺序出现  
8). 所有由 `a` 和 `b` 组成且不含子串 `abb` 的串  
9). 所有由 `a` 和 `b` 组成且不含子序列 `abb` 的串

### 解

1). 包含 5 个元音的所有小写字母串，这些串中的元音按顺序出现  
```
str  -> char* a (a|char)* e (e|char)* i (i|char)* o (o|char)* u (u|char)*
char -> [bcdfghjklmnpqrstvwxyz]
```

8). 所有由 `a` 和 `b` 组成且不含子串 `abb` 的串  
```
str -> b*(a+b?)*
```
任何一串 `a` 后面可以跟 0 个 或者 1 个 `b`，但不能跟 2 个或以上的 `b`。

9). 所有由 `a` 和 `b` 组成且不含子序列 `abb` 的串
```
str -> (b* a*) | (b* a+ b a*)
```
出现 `a` 之后最多出现 1 个 `b`；