# 编译原理 第 6 章 作业 2

## 1
确定下列声明序列中各个标识符的类型和相对地址：
```
float x；
record { float x; float y } p;
record { int tag; float x; float y } q;
```

### 解

<!-- 构造 SDT 如下：
```
S ->                { top = new Evn(); offset = 0;            }
        D 
D ->    T id;       { top.put(id.lexeme, T.type, offset);
                  offset += T.width                       }
        D1
D ->    ε
T ->    int         { T.type = interget; T.width = 4;         }
T ->    float       { T.type = float; T.width = 8;            }
T ->    record '{'  { Evn.push(top), top = new Evn();
                      Stack.push(offset), offset = 0;         }
        D '}'       { T.type = record(top); T.width = offset;
                      top = Evn.top(); offset = Stack.pop();  }
``` -->

故标识符的类型和相对地址如下：
| 标识符  |    类型    | 大小  | 环境  | 相对地址 |
| :-----: | :--------: | :---: | :---: | :------: |
|   `x`   |  `float`   |  `8`  |  `0`  |   `0`    |
|  `p.x`  |  `float`   |  `8`  |  `1`  |   `0`    |
|  `p.y`  |  `float`   |  `8`  |  `1`  |   `8`    |
|   `p`   | `record()` | `16`  |  `0`  |   `8`    |
| `q.tag` |   `int`    |  `4`  |  `2`  |   `0`    |
|  `q.x`  |  `float`   |  `8`  |  `2`  |   `4`    |
|  `q.y`  |  `float`   |  `8`  |  `2`  |   `12`   |
|   `q`   | `record()` | `20`  |  `0`  |   `24`   |

## 2
使用图 6-22 的翻译方案翻译赋值语句 `x = a[b[i][j]][c[k]]`。

### 解

<img src="./2-2-1.svg">

得到如下三地址代码：
```
t1 = i * bi_width
t2 = j * bj_width
t3 = t1 + t2
t4 = b[t3]
t5 = t4 * abijwidth
t6 = c_width
t7 = c[t6]
t8 = t6 * ckwidth
t9 = t5 + t8
t10 = a[t9]
x = t10
```


## 3
假定图 6-26 中的函数 widen 可以处理图 6-26a 的层次结构中的所有类型，

翻译下列表达式。假定 c 和 d 是 char 型，s 和 t 是 short 型，i 和 j 是 int 型，x 是 float 型。

(1) `x = s + c`  
(2) `i = s + c`  
(3) `x = ( s + c ) * ( t + d )`  

### 解

(1) 
```
t1 = (int) s
t2 = (int) c
t3 = t1 + t2
x = (float) t3
```

(2)
```
t1 = (int) s
t2 = (int) c
i = t1 + t2
```

(3)
```
t1 = (int) s
t2 = (int) c
t3 = t1 + t2
t4 = (int) t
t5 = (int) d
t6 = t4 + t5
t7 = t3 * t6
x = (float) t7
```