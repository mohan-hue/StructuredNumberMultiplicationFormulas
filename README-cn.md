# 前缀、后缀与平方拆分乘法公式说明

[简体中文](./README-cn.md)|[English](./README.md)

这份 `README.md` 用小白也能看懂的方式，整理三类乘法拆分公式：

1. **前缀相同公式**
2. **后缀相同公式**
3. **平方公式**
4. **二进制版本**
5. **使用途径、适用场景和限制**

这些公式的核心思想是：

> 当两个数有相同结构时，可以把部分交叉项合并，从而减少计算量。

不过要注意：这些公式不是万能加速算法。它们适合**有结构的数字**，例如：

- 前面很长一段相同；

- 后面很长一段相同；

- 数字接近 
    $$
    10^n
    $$

- 数字接近
    $$
    2^n
    $$

- 计算的是平方；

- 在二进制里有长共同前缀或长共同后缀。

如果数字是完全随机的大数，这些公式就用不了了，主要是用于快速口算或者笔算带结构的数。

---

## 目录

- [1. 基础概念：什么叫拆分](#1-基础概念什么叫拆分)
- [2. 前缀相同公式](#2-前缀相同公式)
- [3. 后缀相同公式](#3-后缀相同公式)
- [4. 平方公式](#4-平方公式)
- [5. 二进制版本](#5-二进制版本)
- [6. 前缀 + 后缀组合公式](#6-前缀--后缀组合公式)
- [7. 使用途径：什么时候该用](#7-使用途径什么时候该用)
- [8. 和 NTT / FFT 的关系](#8-和-ntt--fft-的关系)
- [9. 常见错误](#9-常见错误)
- [10. 总结](#10-总结)

---

# 1. 基础概念：什么叫拆分

一个数可以拆成：

```text
前面部分 × 位权 + 后面部分
```

例如：

$$
123456 = 123 \times 1000 + 456
$$

因为：

$$
1000 = 10^3
$$

所以也可以写成：

$$
123456 = 123 \times 10^3 + 456
$$

这里：

| 符号 | 含义 |
|---|---|
| `123` | 前面的部分 |
| `456` | 后面的部分 |
| `10^3` | 位权，表示后面有 3 位 |
| `3` | 后缀长度 |

再比如：

$$
135 = 13 \times 10 + 5
$$

因为后面只剩 1 位，所以位权是：

$$
10^1 = 10
$$

---

# 2. 前缀相同公式

## 2.1 适用情况

如果两个数有相同前缀，比如：

```text
123456
123789
```

它们共同前缀是：

```text
123
```

后面分别是：

```text
456
789
```

就可以使用前缀相同公式。

---

## 2.2 公式

设两个数是：

$$
\begin{gathered}
A = P \times B^k + a \\
C = P \times B^k + c
\end{gathered}
$$

那么：

$$
A \times C = P^2 \times B^{2k} + P(a + c) \times B^k + ac
$$

也可以写得更像口算：

$$
(PX + a)(PX + c) = P^2X^2 + P(a + c)X + ac
$$

其中：

$$
X = B^k
$$

---

## 2.3 每个字母是什么意思？

| 符号 | 含义 |
|---|---|
| `A` | 第一个数 |
| `C` | 第二个数 |
| P | 两个数共同的前缀 |
| `a` | 第一个数去掉共同前缀后，剩下的后缀 |
| `c` | 第二个数去掉共同前缀后，剩下的后缀 |
| B | 使用的进制底数，十进制是 `10`，二进制是 `2` |
| k | 后面剩下多少位 |
| B^k | 位权，表示把前缀往左推 k 位 |

十进制中：

$$
B = 10
$$

所以：

$$
B^k = 10^k
$$

二进制中：

$$
B = 2
$$

所以：

$$
B^k = 2^k
$$

---

## 2.4 为什么公式成立？

从：

$$
\begin{gathered}
A = P \times B^k + a \\
C = P \times B^k + c
\end{gathered}
$$

开始。

相乘：

$$
A \times C = (P \times B^k + a)(P \times B^k + c)
$$

展开：

$$
\begin{gathered}
= P \times B^k \times P \times B^k \\
+ P \times B^k \times c \\
+ a \times P \times B^k \\
+ ac
\end{gathered}
$$

整理：

$$
\begin{gathered}
= P^2 \times B^{2k} \\
+ Pc \times B^k \\
+ Pa \times B^k \\
+ ac
\end{gathered}
$$

中间两项可以合并：

$$
Pc \times B^k + Pa \times B^k = P(a + c) \times B^k
$$

所以：

$$
A \times C = P^2 \times B^{2k} + P(a + c) \times B^k + ac
$$

---

## 2.5 验证例子 1：13 × 12

$$
\begin{gathered}
13 = 1 \times 10 + 3 \\
12 = 1 \times 10 + 2
\end{gathered}
$$

所以：

| 符号 | 数值 |
|---|---|
| P | 1 |
| B | `10` |
| k | 1 |
| `a` | `3` |
| `c` | `2` |

套公式：

$$
\begin{gathered}
13 \times 12 \\
= 1^2 \times 10^2 + 1(3 + 2) \times 10 + 3 \times 2 \\
= 100 + 50 + 6 \\
= 156
\end{gathered}
$$

验证：

$$
13 \times 12 = 156
$$

正确。

---

## 2.6 验证例子 2：135 × 136

$$
\begin{gathered}
135 = 13 \times 10 + 5 \\
136 = 13 \times 10 + 6
\end{gathered}
$$

共同前缀是：

```text
13
```

所以：

| 符号 | 数值 |
|---|---|
| P | `13` |
| B | `10` |
| k | 1 |
| `a` | `5` |
| `c` | `6` |

套公式：

$$
\begin{gathered}
135 \times 136 \\
= 13^2 \times 10^2 + 13(5 + 6) \times 10 + 5 \times 6
\end{gathered}
$$

计算：

$$
\begin{gathered}
13^2 = 169 \\
169 \times 100 = 16900 \\
5 + 6 = 11 \\
13 \times 11 \times 10 = 1430 \\
5 \times 6 = 30
\end{gathered}
$$

相加：

$$
16900 + 1430 + 30 = 18360
$$

所以：

$$
135 \times 136 = 18360
$$

---

## 2.7 验证例子 3：123456 × 123789

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
123789 = 123 \times 10^3 + 789
\end{gathered}
$$

所以：

| 符号 | 数值 |
|---|---|
| P | `123` |
| B | `10` |
| k | `3` |
| `a` | `456` |
| `c` | `789` |

套公式：

$$
\begin{gathered}
123456 \times 123789 \\
= 123^2 \times 10^6 \\
+ 123(456 + 789) \times 10^3 \\
+ 456 \times 789
\end{gathered}
$$

计算：

$$
\begin{gathered}
123^2 = 15129 \\
15129 \times 10^6 = 15129000000 \\
456 + 789 = 1245 \\
123 \times 1245 = 153135 \\
153135 \times 10^3 = 153135000 \\
456 \times 789 = 359784
\end{gathered}
$$

相加：

$$
15129000000 + 153135000 + 359784 = 15282494784
$$

所以：

$$
123456 \times 123789 = 15282494784
$$

---

# 3. 后缀相同公式

## 3.1 适用情况

如果两个数有相同后缀，比如：

```text
123456
89456
```

它们共同后缀是：

```text
456
```

前面的部分分别是：

```text
123
89
```

就可以使用后缀相同公式。

---

## 3.2 公式

设两个数是：

$$
\begin{gathered}
A = u \times B^k + S \\
C = v \times B^k + S
\end{gathered}
$$

那么：

$$
A \times C = uv \times B^{2k} + (u + v)S \times B^k + S^2
$$

也可以写成：

$$
(uX + S)(vX + S) = uvX^2 + (u + v)SX + S^2
$$

其中：

$$
X = B^k
$$

---

## 3.3 每个字母是什么意思？

| 符号 | 含义 |
|---|---|
| `A` | 第一个数 |
| `C` | 第二个数 |
| `u` | 第一个数去掉共同后缀后，前面的部分 |
| `v` | 第二个数去掉共同后缀后，前面的部分 |
| S | 两个数共同的后缀 |
| B | 使用的进制底数，十进制是 `10`，二进制是 `2` |
| k | 共同后缀的位数 |
| `B^k` | 位权，表示前面的部分要左移 $k$ 位 |

---

## 3.4 为什么公式成立？

从：

$$
\begin{gathered}
A = u \times B^k + S \\
C = v \times B^k + S
\end{gathered}
$$

开始。

相乘：

$$
A \times C = (u \times B^k + S)(v \times B^k + S)
$$

展开：

$$
\begin{gathered}
= u \times B^k \times v \times B^k \\
+ u \times B^k \times S \\
+ v \times B^k \times S \\
+ S^2
\end{gathered}
$$

整理：

$$
\begin{gathered}
= uv \times B^{2k} \\
+ uS \times B^k \\
+ vS \times B^k \\
+ S^2
\end{gathered}
$$

中间两项合并：

$$
uS \times B^k + vS \times B^k = (u + v)S \times B^k
$$

所以：

$$
A \times C = uv \times B^{2k} + (u + v)S \times B^k + S^2
$$

---

## 3.5 验证例子 1：15 × 25

$$
\begin{gathered}
15 = 1 \times 10 + 5 \\
25 = 2 \times 10 + 5
\end{gathered}
$$

共同后缀是：

```text
5
```

所以：

| 符号 | 数值 |
|---|---|
| `u` | 1 |
| `v` | `2` |
| S | `5` |
| B | `10` |
| k | $1$ |

套公式：

$$
\begin{gathered}
15 \times 25 \\
= 1 \times 2 \times 10^2 + (1 + 2) \times 5 \times 10 + 5^2 \\
= 200 + 150 + 25 \\
= 375
\end{gathered}
$$

验证：

$$
15 \times 25 = 375
$$

正确。

---

## 3.6 验证例子 2：123456 × 89456

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
89456 = 89 \times 10^3 + 456
\end{gathered}
$$

共同后缀是：

```text
456
```

所以：

| 符号 | 数值 |
|---|---|
| `u` | `123` |
| `v` | `89` |
| S | `456` |
| B | `10` |
| k | `3` |

套公式：

$$
\begin{gathered}
123456 \times 89456 \\
= 123 \times 89 \times 10^6 \\
+ (123 + 89) \times 456 \times 10^3 \\
+ 456^2
\end{gathered}
$$

计算：

$$
\begin{gathered}
123 \times 89 = 10947 \\
10947 \times 10^6 = 10947000000 \\
123 + 89 = 212 \\
212 \times 456 = 96672 \\
96672 \times 10^3 = 96672000 \\
456^2 = 207936
\end{gathered}
$$

相加：

$$
10947000000 + 96672000 + 207936 = 11043879936
$$

所以：

$$
123456 \times 89456 = 11043879936
$$

---

## 3.7 验证例子 3：10056 × 56

这个例子说明：短数字可以在左边补 0。

```text
56 可以看成 00056
```

所以：

$$
\begin{gathered}
10056 = 1 \times 10^4 + 56 \\
00056 = 0 \times 10^4 + 56
\end{gathered}
$$

共同后缀是：

```text
0056
```

数值上：

$$
S = 56
$$

共同后缀长度是：

$$
k = 4
$$

因为看成 4 位：

```text
0056
```

所以：

| 符号 | 数值 |
|---|---|
| `u` | $1$ |
| `v` | $0$ |
| $S$ | `56` |
| $B$ | `10` |
| $k$ | `4` |

套公式：

$$
\begin{gathered}
10056 \times 56 \\
= 1 \times 0 \times 10^8 \\
+ (1 + 0) \times 56 \times 10^4 \\
+ 56^2
\end{gathered}
$$

计算：

$$
0 + 560000 + 3136 = 563136
$$

所以：

$$
10056 \times 56 = 563136
$$

---

# 4. 平方公式

平方就是一个数乘自己。

例如：

$$
135^2 = 135 \times 135
$$

平方有特殊性，因为交叉项会重复。

---

## 4.1 公式

如果：

$$
A = P \times B^k + a
$$

那么：

$$
A^2 = P^2 \times B^{2k} + 2Pa \times B^k + a^2
$$

也可以写成：

$$
(PX + a)^2 = P^2X^2 + 2PaX + a^2
$$

其中：

$$
X = B^k
$$

---

## 4.2 每个字母是什么意思？

| 符号 | 含义 |
|---|---|
| `A` | 要平方的数 |
| P | 前面的部分 |
| `a` | 后面的部分 |
| B | 进制底数 |
| k | 后面的部分占多少位 |
| `B^k` | 位权 |

---

## 4.3 验证例子：135²

$$
135 = 13 \times 10 + 5
$$

所以：

| 符号 | 数值 |
|---|---|
| P | `13` |
| `a` | `5` |
| B | `10` |
| k | 1 |

套公式：

$$
\begin{gathered}
135^2 \\
= 13^2 \times 10^2 + 2 \times 13 \times 5 \times 10 + 5^2
\end{gathered}
$$

计算：

$$
\begin{gathered}
13^2 = 169 \\
169 \times 100 = 16900 \\
2 \times 13 \times 5 \times 10 = 1300 \\
5^2 = 25
\end{gathered}
$$

相加：

$$
16900 + 1300 + 25 = 18225
$$

所以：

$$
135^2 = 18225
$$

---

# 5. 二进制版本

前面的公式不只适用于十进制，也适用于二进制。

十进制：

$$
B = 10
$$

二进制：

$$
B = 2
$$

计算机内部使用二进制，所以二进制版本非常重要。

---

## 5.1 为什么二进制里乘 2^k 很快？

在十进制里：

$$
123 \times 10^3 = 123000
$$

相当于在十进制后面补 3 个 0。

在二进制里：

$$
101_2 \times 2^3 = 101000_2
$$

相当于在二进制后面补 3 个 0。

也就是：

$$
5 \times 8 = 40
$$

因为：

$$
\begin{gathered}
101_2 = 5 \\
101000_2 = 40
\end{gathered}
$$

计算机中：

$$
x \times 2^k
$$

就是左移：

```text
x << k
```

例如：

$$
5 << 3 = 40
$$

所以在二进制里，乘 `2^k` 非常便宜。

---

## 5.2 二进制前缀公式

如果：

$$
\begin{gathered}
A = P \times 2^k + a \\
C = P \times 2^k + c
\end{gathered}
$$

那么：

$$
A \times C = P^2 \times 2^{2k} + P(a + c) \times 2^k + ac
$$

---

## 5.3 二进制前缀例子

设：

$$
\begin{gathered}
A = 110101_2 \\
C = 110011_2
\end{gathered}
$$

它们共同前缀是：

$$
110_2
$$

后面各剩 3 位：

$$
\begin{gathered}
101_2 \\
011_2
\end{gathered}
$$

所以：

$$
\begin{gathered}
A = 110_2 \times 2^3 + 101_2 \\
C = 110_2 \times 2^3 + 011_2
\end{gathered}
$$

换成十进制：

$$
\begin{gathered}
110_2 = 6 \\
101_2 = 5 \\
011_2 = 3 \\
2^3 = 8
\end{gathered}
$$

所以：

$$
\begin{gathered}
A = 6 \times 8 + 5 = 53 \\
C = 6 \times 8 + 3 = 51
\end{gathered}
$$

套公式：

$$
\begin{gathered}
53 \times 51 \\
= 6^2 \times 2^6 + 6(5 + 3) \times 2^3 + 5 \times 3
\end{gathered}
$$

计算：

$$
\begin{gathered}
6^2 \times 64 = 36 \times 64 = 2304 \\
6(5 + 3) \times 8 = 6 \times 8 \times 8 = 384 \\
5 \times 3 = 15
\end{gathered}
$$

相加：

$$
2304 + 384 + 15 = 2703
$$

验证：

$$
53 \times 51 = 2703
$$

---

## 5.4 二进制后缀公式

如果：

$$
\begin{gathered}
A = u \times 2^k + S \\
C = v \times 2^k + S
\end{gathered}
$$

那么：

$$
A \times C = uv \times 2^{2k} + (u + v)S \times 2^k + S^2
$$

这里：

```text
S
```

表示两个二进制数相同的低位部分。

---

## 5.5 二进制后缀例子

设：

$$
\begin{gathered}
A = 101101_2 \\
C = 111101_2
\end{gathered}
$$

它们共同后缀是：

$$
1101_2
$$

所以：

$$
\begin{gathered}
A = 10_2 \times 2^4 + 1101_2 \\
C = 11_2 \times 2^4 + 1101_2
\end{gathered}
$$

换成十进制：

$$
\begin{gathered}
10_2 = 2 \\
11_2 = 3 \\
1101_2 = 13 \\
2^4 = 16
\end{gathered}
$$

所以：

$$
\begin{gathered}
A = 2 \times 16 + 13 = 45 \\
C = 3 \times 16 + 13 = 61
\end{gathered}
$$

套公式：

$$
\begin{gathered}
45 \times 61 \\
= 2 \times 3 \times 2^8 + (2 + 3) \times 13 \times 2^4 + 13^2
\end{gathered}
$$

计算：

$$
\begin{gathered}
2 \times 3 \times 256 = 1536 \\
(2 + 3) \times 13 \times 16 = 5 \times 13 \times 16 = 1040 \\
13^2 = 169
\end{gathered}
$$

相加：

$$
1536 + 1040 + 169 = 2745
$$

验证：

$$
45 \times 61 = 2745
$$

---

## 5.6 接近 2^n 的数

如果两个数都接近：

$$
2^n
$$

例如：

$$
\begin{gathered}
260 = 2^8 + 4 \\
270 = 2^8 + 14
\end{gathered}
$$

那么：

$$
\begin{gathered}
260 \times 270 \\
= (2^8 + 4)(2^8 + 14) \\
= 2^16 + (4 + 14)2^8 + 4 \times 14
\end{gathered}
$$

计算：

$$
\begin{gathered}
2^16 = 65536 \\
(4 + 14)2^8 = 18 \times 256 = 4608 \\
4 \times 14 = 56
\end{gathered}
$$

相加：

$$
65536 + 4608 + 56 = 70200
$$

所以：

$$
260 \times 270 = 70200
$$

这种结构在计算机中很有用，因为 2^n 对应二进制里的 1 后面跟很多个 0。

---

# 6. 前缀 + 后缀组合公式

有时两个数既有共同前缀，又有共同后缀。

例如：

$$
\begin{gathered}
A = P | x | S \\
C = P | y | S
\end{gathered}
$$

其中：

| 符号 | 含义 |
|---|---|
| P | 共同前缀 |
| S | 共同后缀 |
| x | 第一个数中间不同部分 |
| y | 第二个数中间不同部分 |

如果使用底数 B，共同后缀长度是 `s` 位，中间部分长度是 `r` 位，那么：

$$
\begin{gathered}
A = P \times B^{r+s} + x \times B^s + S \\
C = P \times B^{r+s} + y \times B^s + S
\end{gathered}
$$

展开后：

$$
\begin{gathered}
A \times C 
= P^2 \times B^{2r+2s} 
+ P(x + y) \times B^{r+2s} 
+ xy \times B^{2s} 
+ 2PS \times B^{r+s} 
+ (x + y)S \times B^s 
+ S^2
\end{gathered}
$$

这个公式比较长，但意思很清楚：

```text
共同前缀产生 P^2
共同后缀产生 S^2
中间不同部分产生 xy
前缀和中间有交叉项
前缀和后缀有交叉项
中间和后缀有交叉项
```

这个公式适合：

```text
两个数前面和后面都很像，只有中间一小段不同
```

如果中间不同部分很短，这种拆法可能有优势。

---

# 7. 使用途径：什么时候该用

## 7.1 适合前缀公式的情况

当前面很长一段相同时：

```text
123456789000001
123456789000037
```

可以写成：

$$
\begin{gathered}
P \times 10^k + a \\
P \times 10^k + c
\end{gathered}
$$

此时适合前缀公式。

适合场景：

```text
共同前缀很长，后面不同部分很短
```

---

## 7.2 适合后缀公式的情况

当后面很长一段相同时：

```text
123000000000999
456000000000999
```

可以写成：

$$
\begin{gathered}
u \times 10^k + S \\
v \times 10^k + S
\end{gathered}
$$

此时适合后缀公式。

适合场景：

```text
共同后缀很长，前面不同部分较短
```

---

## 7.3 适合平方公式的情况

当两个数完全相同时：

$$
A \times A = A^2
$$

适合平方公式或专门的平方算法。

平方比普通乘法快，因为：

$$
a_i \times a_j
$$

和：

$$
a_j \times a_i
$$

是重复的，可以合并成：

$$
2 \times a_i \times a_j
$$

---

## 7.4 适合二进制公式的情况

二进制版本适合计算机，因为：

$$
\times 2^k
$$

就是：

```text
左移 k 位
```

适合场景：

```text
接近 2^n
接近 2^n - 1
二进制共同前缀很长
二进制共同后缀很长
```

例如：

$$
\begin{gathered}
A = 2^n + a \\
C = 2^n + c
\end{gathered}
$$

则：

$$
A \times C = 2^{2n} + (a + c)2^n + ac
$$

如果 `a` 和 `c` 很小，这会非常快。

---

## 7.5 完全随机大数的情况

如果数字是完全随机的大数，这些公式就用不了了，主要是用于快速口算或者笔算带结构的数。

此时更适合使用：

```text
普通乘法
Karatsuba
Toom-Cook
NTT
FFT
```

---

# 8. 常见错误

## 8.1 误以为只要开头一样就一定快

例如：

$$
135 \times 186
$$

共同前缀只有：

```text
1
```

虽然可以写成：

$$
\begin{gathered}
135 = 1 \times 100 + 35 \\
186 = 1 \times 100 + 86
\end{gathered}
$$

套公式：

$$
135 \times 186 = 1^2 \times 10000 + 1(35 + 86) \times 100 + 35 \times 86
$$

结果是对的，但最后还要算：

$$
35 \times 86
$$

所以并不一定更快。

共同前缀太短时，优势很小。

---

## 8.2 误以为可以省掉所有交叉项

乘法中交叉项不会消失，只能被合并或批量处理。

例如：

$$
(a + b)(c + d) = ac + ad + bc + bd
$$

中间的：

$$
ad + bc
$$

就是交叉项。

前缀/后缀公式的作用是：

```text
当某些部分相同时，把两个交叉项合并成一个乘法
```

不是让交叉项不存在。

---

## 8.3 前缀没有对齐就直接套公式

例如：

```text
123456
12389
```

它们都以 `123` 开头，但位置不同：

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
12389  = 123 \times 10^2 + 89
\end{gathered}
$$

一个是：

$$
123 \times 10^3
$$

另一个是：

$$
123 \times 10^2
$$

位权不一样，不能直接套简单前缀公式。

可以把短的数右边补 0 对齐：

$$
12389 \times 10 = 123890
$$

然后算：

$$
123456 \times 123890
$$

最后再除以 `10`。

但这不一定更快，只是数学上可行。

---

## 8.4 后缀不是从最右边连续相同

后缀必须是从最右边开始连续相同。

例如：

```text
123456
789456
```

共同后缀是：

```text
456
```

可以用后缀公式。

但是：

```text
123456
789356
```

虽然中间有些数字相同，但从右边连续相同的只有：

```text
56
```

所以共同后缀只能取 `56`。

---

# 9. 总结

## 9.1 前缀相同公式

$$
\begin{gathered}
(P \times B^k + a)(P \times B^k + c) \\
= P^2 \times B^{2k} + P(a + c) \times B^k + ac
\end{gathered}
$$

适合：

```text
两个数有长共同前缀
```

---

## 9.2 后缀相同公式

$$
\begin{gathered}
(u \times B^k + S)(v \times B^k + S) \\
= uv \times B^{2k} + (u + v)S \times B^k + S^2
\end{gathered}
$$

适合：

```text
两个数有长共同后缀
```

---

## 9.3 平方公式

$$
\begin{gathered}
(P \times B^k + a)^2 \\
= P^2 \times B^{2k} + 2Pa \times B^k + a^2
\end{gathered}
$$

适合：

```text
计算一个数的平方
```

---

## 9.4 二进制版本

把 `B = 10` 换成 `B = 2`。

前缀：

$$
\begin{gathered}
(P \times 2^k + a)(P \times 2^k + c) \\
= P^2 \times 2^{2k} + P(a + c) \times 2^k + ac
\end{gathered}
$$

后缀：

$$
\begin{gathered}
(u \times 2^k + S)(v \times 2^k + S) \\
= uv \times 2^{2k} + (u + v)S \times 2^k + S^2
\end{gathered}
$$

在计算机中：

```text
× 2^k = 左移 k 位
```

所以二进制公式更适合机器计算。

---

## 9.5 最重要的一句话

```text
小数字里，它们是心算技巧。
大数字里，它们是结构优化。
二进制里，它们可以变成移位、加减和更小的乘法。
完全随机的大数里，它们通常用不上。
有结构的大数里，它们可能非常快。
```
