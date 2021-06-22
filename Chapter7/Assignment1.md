# 编译原理 第 7 章 作业 1
> 徐泽凡 2018K8009929037

## 1
下面是两个 C 语言函数 `f` 和 `g` 的概述：
```c
int f ( int x ) { int i; ... return i + 1; ... }
int g ( int y ) { int j; ... f ( j + 1 ); ... }
```
也就是说，函数 `g` 调用 `f`。

画出在 `g` 调用 `f` 而 `f` 即将返回时，运行时刻栈中从 `g` 的活动记录开始的顶端部分。

你可以只考虑返回值、参数、控制链以及存放局部数据的空间。你不用考虑存放的机器状态，也不用考虑没有在代码中显示的局部值和临时值。

但是你应该指出：  
1）哪个函数在栈中为各个元素创建了所使用的空间？  
2）哪个函数写入了各个元素的值？  
3）这些元素属于哪个活动记录？  

### 解

如图：

<img src="./1-1.svg" style="width: 37.5em;">

<div style="page-break-after: always;"></div>

## 2
图 7-15 中给出了一个按照非标准方式计算 Fibonacci 数的 ML 语言的函数 `main`。函数 `fib0` 将计算第 $n$ 个 Fibonacci 数（$n \geq 0$）。嵌套在 `fib0` 中的是 `fib1`，它假设 $n \geq 2$ 并计算第 $n$ 个 Fibonacci 数。嵌套在 `fib1` 中的是 `fib2`，它假设 $n \geq 4$。请注意，`fib1` 和 `fib2` 都不需要检查基本情况。我们考虑从对 `main` 的调用开始，直到（对 `fib0(1)` 的）第一次调用即将返回的时段，请描述出当时的活动记录栈，并给出栈中的各个活动记录的访问链。

图 7-15 Nested functions computing Fibonacci numbers

```ML
fun main() =
    let 
        fun fib0(n) = 
            let
                fun fib1(n) = 
                    let
                        fun fib2(n) = fib1(n-1) + fib1(n-2)
                    in
                        if n >= 4 then fib2(n)
                        else fib0(n-1) + fib0(n-2)
                    end
            in
                if n >= 2 then fib1(n)
                else 1
            end
    in
        fib0(4)
    end;
```

### 解

如下页左图。

## 3
假设我们使用 display 表来实现图 7-15 中的函数。请给出 `fib0(1)` 的第一次调用即将返回时的 display 表。同时指明那时在栈中的各个活动记录中保存的 display 表条目。

### 解

如下页右图。

<div style="page-break-after: always;"></div>

左为第 2 题图，右为第 3 题图。

<img src="./1-2.svg" style="height: 43.2em; margin-right: 8em;">
<img src="./1-3.svg" style="height: 43.2em;">