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