# 编译原理 第 8 章 作业 1

## 1
8.2.5：假设 `n` 在一个内存位置中，为下面的语句序列生成代码，并计算生成的目标代码的代价：

```
    s = 0
    i = 0
L1: if i > n goto L2
    s = s + i
    i = i + 1
    goto L1
L2:
```

### 解

生成的代码如下：
```
    LD    R1, #0      // 2    R1 = s = 0
    LD    R2, #0      // 2    R2 = i = 0
    LD    R3, n       // 2    R3 = n
L1: SUB   R4, R2, R3  // 1    R4 = R2 - R3 = i - n
    BGTZ  R4, L2      // 2    if R4 > 0 Goto L2
    ADD   R1, R1, R2  // 1    R1 = s = s + i
    ADD   R2, R2, #1  // 2    R2 = i = i + 1
    BR    L1          // 2    Goto L1
L2:
```

上述代码注释的第一部分即为代价，总代价为 14。


## 2
8.3.3：假设使用栈式分配，且假设 `a` 和 `b` 都是元素大小为 `4` 字节的数组，为下面的三地址语句生成代码：
```
x = a[i]
y = b[j]
a[i] = y
b[j] = x
```

<div style="page-break-after: always;"></div>

### 解

生成的代码如下：
```
    LD    R1, i       // R1 = i
    MUL   R1, R1, 4   // R1 = R1 * 4
    ADD   R1, R1, SP  // R1 = R1 + SP
    LD    R2, a(R1)   // R2 = contents(a + contents(R1))
    ST    x(SP), R2   // x = R2

    LD    R3, j       // R3 = j
    MUL   R3, R3, 4   // R3 = R3 * 4
    ADD   R3, R3, SP  // R3 = R3 + SP
    LD    R4, b(R3)   // R4 = contents(a + contents(R3))
    ST    y(SP), R4   // y = R4

    ST    a(R1), R4   // a[i] = y
    
    ST    b(R3), R2   // b[j] = x
```