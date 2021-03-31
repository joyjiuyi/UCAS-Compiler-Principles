# 编译原理 第 2 章 作业 2

## 练习 2.8.1
C语言和Java语言中的for语句具有如下形式：
```
for ( expr1 ; expr2 ; expr3 ) stmt
```
第一个表达式在循环之前执行，它通常被用来初始化循环下标。第二个表达式是一个测试，它在循环的每次迭代之前执行。如果这个表达式的结果变成 `0`，就退出循环。循环本身可以被看作语句 `{ stmt expr3 ;}`。第三个表达式在每一次迭代的末尾执行，它通常用来使循环下标递增，故 `for` 语句的含义类似于
```
expr1 ; while (expr2) { stmt expr3; }
```
仿照图 2-43 中的类 `If`，为 `for` 语句定义一个类 `For`。

### 解

类 `For` 的定义如下：
```
class For extends Stmt {
    Expr E1, E2, E3;
    Stmt S;
    public For(Expr expr1, Expr expr2, Expr expr3, Stmt stmt) {
        E1 = expr1;
        E2 = expr2;
        E3 = expr3;
        S = stmt;
        start = newlabel();
        after = newlabel();
    }
    public void gen() {
        E1.gen();
        emit(start + ":");
        Expr n = E2.rvalue();
        emit("ifFalse " + n.toString() + " goto " + after);
        S.gen();
        E3.gen();
        emit("goto " + start);
        emit(after + ":");
    }
}
```