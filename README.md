# Prefix, Suffix, and Squaring Decomposition Formulas for Multiplication

[简体中文](./README-cn.md)|[English](./README.md)

This English `README.md` explains three multiplication decomposition formulas in a beginner-friendly way:

1. **Common-prefix formula**
2. **Common-suffix formula**
3. **Squaring formula**
4. **Binary versions**
5. **Use cases, suitable scenarios, and limitations**

The core idea is:

> When two numbers share the same structure, some cross terms can be combined, reducing the amount of calculation.

However, these formulas are **not universal acceleration algorithms**. They are suitable for **structured numbers**, such as:

- two numbers sharing a long prefix;
- two numbers sharing a long suffix;
- numbers close to

$$
10^n
$$

- numbers close to

$$
2^n
$$

- squaring a number;
- binary numbers with long common prefixes or long common suffixes.

If the numbers are completely random large numbers, these formulas cannot really be used. They are mainly useful for fast mental calculation or written calculation of structured numbers.

---

## Table of Contents

- [1. Basic Idea: What Does Decomposition Mean?](#1-basic-idea-what-does-decomposition-mean)
- [2. Common-Prefix Formula](#2-common-prefix-formula)
- [3. Common-Suffix Formula](#3-common-suffix-formula)
- [4. Squaring Formula](#4-squaring-formula)
- [5. Binary Versions](#5-binary-versions)
- [6. Combined Prefix + Suffix Formula](#6-combined-prefix--suffix-formula)
- [7. Use Cases: When Should These Formulas Be Used?](#7-use-cases-when-should-these-formulas-be-used)
- [8. Relationship with NTT / FFT](#8-relationship-with-ntt--fft)
- [9. Common Mistakes](#9-common-mistakes)
- [10. Summary](#10-summary)

---

# 1. Basic Idea: What Does Decomposition Mean?

A number can be split into:

```text
front part × place value + back part
```

For example:

$$
123456 = 123 \times 1000 + 456
$$

Because:

$$
1000 = 10^3
$$

we can also write:

$$
123456 = 123 \times 10^3 + 456
$$

Here:

| Symbol | Meaning |
|---|---|
| `123` | the front part |
| `456` | the back part |
| `10^3` | the place value, meaning there are 3 digits after the front part |
| `3` | the suffix length |

Another example:

$$
135 = 13 \times 10 + 5
$$

Since only 1 digit remains at the back, the place value is:

$$
10^1 = 10
$$

---

# 2. Common-Prefix Formula

## 2.1 When It Applies

If two numbers share the same prefix, for example:

```text
123456
123789
```

their common prefix is:

```text
123
```

and the remaining suffixes are:

```text
456
789
```

Then we can use the common-prefix formula.

---

## 2.2 Formula

Suppose the two numbers are:

$$
\begin{gathered}
A = P \times B^k + a \\
C = P \times B^k + c
\end{gathered}
$$

Then:

$$
A \times C = P^2 \times B^{2k} + P(a + c) \times B^k + ac
$$

A more mental-arithmetic-friendly version is:

$$
(PX + a)(PX + c) = P^2X^2 + P(a + c)X + ac
$$

where:

$$
X = B^k
$$

---

## 2.3 What Does Each Letter Mean?

| Symbol | Meaning |
|---|---|
| `A` | the first number |
| `C` | the second number |
| `P` | the common prefix of the two numbers |
| `a` | the suffix of the first number after removing the common prefix |
| `c` | the suffix of the second number after removing the common prefix |
| `B` | the base. In decimal, `B = 10`; in binary, `B = 2` |
| `k` | how many digits remain after the common prefix |
| `B^k` | the place value, pushing the prefix left by `k` digits |

In decimal:

$$
B = 10
$$

so:

$$
B^k = 10^k
$$

In binary:

$$
B = 2
$$

so:

$$
B^k = 2^k
$$

---

## 2.4 Why Does the Formula Work?

Start with:

$$
\begin{gathered}
A = P \times B^k + a \\
C = P \times B^k + c
\end{gathered}
$$

Multiply them:

$$
A \times C = (P \times B^k + a)(P \times B^k + c)
$$

Expand:

$$
\begin{gathered}
= P \times B^k \times P \times B^k \\
+ P \times B^k \times c \\
+ a \times P \times B^k \\
+ ac
\end{gathered}
$$

Rearrange:

$$
\begin{gathered}
= P^2 \times B^{2k} \\
+ Pc \times B^k \\
+ Pa \times B^k \\
+ ac
\end{gathered}
$$

The two middle terms can be combined:

$$
Pc \times B^k + Pa \times B^k = P(a + c) \times B^k
$$

Therefore:

$$
A \times C = P^2 \times B^{2k} + P(a + c) \times B^k + ac
$$

---

## 2.5 Example 1: 13 × 12

$$
\begin{gathered}
13 = 1 \times 10 + 3 \\
12 = 1 \times 10 + 2
\end{gathered}
$$

So:

| Symbol | Value |
|---|---|
| `P` | `1` |
| `B` | `10` |
| `k` | `1` |
| `a` | `3` |
| `c` | `2` |

Apply the formula:

$$
\begin{gathered}
13 \times 12 \\
= 1^2 \times 10^2 + 1(3 + 2) \times 10 + 3 \times 2 \\
= 100 + 50 + 6 \\
= 156
\end{gathered}
$$

Verification:

$$
13 \times 12 = 156
$$

Correct.

---

## 2.6 Example 2: 135 × 136

$$
\begin{gathered}
135 = 13 \times 10 + 5 \\
136 = 13 \times 10 + 6
\end{gathered}
$$

The common prefix is:

```text
13
```

So:

| Symbol | Value |
|---|---|
| `P` | `13` |
| `B` | `10` |
| `k` | `1` |
| `a` | `5` |
| `c` | `6` |

Apply the formula:

$$
\begin{gathered}
135 \times 136 \\
= 13^2 \times 10^2 + 13(5 + 6) \times 10 + 5 \times 6
\end{gathered}
$$

Calculate:

$$
\begin{gathered}
13^2 = 169 \\
169 \times 100 = 16900 \\
5 + 6 = 11 \\
13 \times 11 \times 10 = 1430 \\
5 \times 6 = 30
\end{gathered}
$$

Add them:

$$
16900 + 1430 + 30 = 18360
$$

So:

$$
135 \times 136 = 18360
$$

---

## 2.7 Example 3: 123456 × 123789

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
123789 = 123 \times 10^3 + 789
\end{gathered}
$$

So:

| Symbol | Value |
|---|---|
| `P` | `123` |
| `B` | `10` |
| `k` | `3` |
| `a` | `456` |
| `c` | `789` |

Apply the formula:

$$
\begin{gathered}
123456 \times 123789 \\
= 123^2 \times 10^6 \\
+ 123(456 + 789) \times 10^3 \\
+ 456 \times 789
\end{gathered}
$$

Calculate:

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

Add them:

$$
15129000000 + 153135000 + 359784 = 15282494784
$$

So:

$$
123456 \times 123789 = 15282494784
$$

---

# 3. Common-Suffix Formula

## 3.1 When It Applies

If two numbers share the same suffix, for example:

```text
123456
89456
```

their common suffix is:

```text
456
```

and the front parts are:

```text
123
89
```

Then we can use the common-suffix formula.

---

## 3.2 Formula

Suppose the two numbers are:

$$
\begin{gathered}
A = u \times B^k + S \\
C = v \times B^k + S
\end{gathered}
$$

Then:

$$
A \times C = uv \times B^{2k} + (u + v)S \times B^k + S^2
$$

A shorter version is:

$$
(uX + S)(vX + S) = uvX^2 + (u + v)SX + S^2
$$

where:

$$
X = B^k
$$

---

## 3.3 What Does Each Letter Mean?

| Symbol | Meaning |
|---|---|
| `A` | the first number |
| `C` | the second number |
| `u` | the front part of the first number after removing the common suffix |
| `v` | the front part of the second number after removing the common suffix |
| `S` | the common suffix of the two numbers |
| `B` | the base. In decimal, `B = 10`; in binary, `B = 2` |
| `k` | the number of digits in the common suffix |
| `B^k` | the place value, meaning the front part is shifted left by `k` digits |

---

## 3.4 Why Does the Formula Work?

Start with:

$$
\begin{gathered}
A = u \times B^k + S \\
C = v \times B^k + S
\end{gathered}
$$

Multiply them:

$$
A \times C = (u \times B^k + S)(v \times B^k + S)
$$

Expand:

$$
\begin{gathered}
= u \times B^k \times v \times B^k \\
+ u \times B^k \times S \\
+ v \times B^k \times S \\
+ S^2
\end{gathered}
$$

Rearrange:

$$
\begin{gathered}
= uv \times B^{2k} \\
+ uS \times B^k \\
+ vS \times B^k \\
+ S^2
\end{gathered}
$$

The two middle terms can be combined:

$$
uS \times B^k + vS \times B^k = (u + v)S \times B^k
$$

Therefore:

$$
A \times C = uv \times B^{2k} + (u + v)S \times B^k + S^2
$$

---

## 3.5 Example 1: 15 × 25

$$
\begin{gathered}
15 = 1 \times 10 + 5 \\
25 = 2 \times 10 + 5
\end{gathered}
$$

The common suffix is:

```text
5
```

So:

| Symbol | Value |
|---|---|
| `u` | `1` |
| `v` | `2` |
| `S` | `5` |
| `B` | `10` |
| `k` | `1` |

Apply the formula:

$$
\begin{gathered}
15 \times 25 \\
= 1 \times 2 \times 10^2 + (1 + 2) \times 5 \times 10 + 5^2 \\
= 200 + 150 + 25 \\
= 375
\end{gathered}
$$

Verification:

$$
15 \times 25 = 375
$$

Correct.

---

## 3.6 Example 2: 123456 × 89456

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
89456 = 89 \times 10^3 + 456
\end{gathered}
$$

The common suffix is:

```text
456
```

So:

| Symbol | Value |
|---|---|
| `u` | `123` |
| `v` | `89` |
| `S` | `456` |
| `B` | `10` |
| `k` | `3` |

Apply the formula:

$$
\begin{gathered}
123456 \times 89456 \\
= 123 \times 89 \times 10^6 \\
+ (123 + 89) \times 456 \times 10^3 \\
+ 456^2
\end{gathered}
$$

Calculate:

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

Add them:

$$
10947000000 + 96672000 + 207936 = 11043879936
$$

So:

$$
123456 \times 89456 = 11043879936
$$

---

## 3.7 Example 3: 10056 × 56

This example shows that a shorter number can be padded with zeros on the left.

```text
56 can be treated as 00056
```

So:

$$
\begin{gathered}
10056 = 1 \times 10^4 + 56 \\
00056 = 0 \times 10^4 + 56
\end{gathered}
$$

The common suffix is:

```text
0056
```

Numerically:

$$
S = 56
$$

The common suffix length is:

$$
k = 4
$$

because we treat it as 4 digits:

```text
0056
```

So:

| Symbol | Value |
|---|---|
| `u` | `1` |
| `v` | `0` |
| `S` | `56` |
| `B` | `10` |
| `k` | `4` |

Apply the formula:

$$
\begin{gathered}
10056 \times 56 \\
= 1 \times 0 \times 10^8 \\
+ (1 + 0) \times 56 \times 10^4 \\
+ 56^2
\end{gathered}
$$

Calculate:

$$
0 + 560000 + 3136 = 563136
$$

So:

$$
10056 \times 56 = 563136
$$

---

# 4. Squaring Formula

Squaring means multiplying a number by itself.

For example:

$$
135^2 = 135 \times 135
$$

Squaring has a special structure because cross terms repeat.

---

## 4.1 Formula

If:

$$
A = P \times B^k + a
$$

then:

$$
A^2 = P^2 \times B^{2k} + 2Pa \times B^k + a^2
$$

A shorter version is:

$$
(PX + a)^2 = P^2X^2 + 2PaX + a^2
$$

where:

$$
X = B^k
$$

---

## 4.2 What Does Each Letter Mean?

| Symbol | Meaning |
|---|---|
| `A` | the number being squared |
| `P` | the front part |
| `a` | the back part |
| `B` | the base |
| `k` | how many digits the back part has |
| `B^k` | the place value |

---

## 4.3 Example: 135²

$$
135 = 13 \times 10 + 5
$$

So:

| Symbol | Value |
|---|---|
| `P` | `13` |
| `a` | `5` |
| `B` | `10` |
| `k` | `1` |

Apply the formula:

$$
\begin{gathered}
135^2 \\
= 13^2 \times 10^2 + 2 \times 13 \times 5 \times 10 + 5^2
\end{gathered}
$$

Calculate:

$$
\begin{gathered}
13^2 = 169 \\
169 \times 100 = 16900 \\
2 \times 13 \times 5 \times 10 = 1300 \\
5^2 = 25
\end{gathered}
$$

Add them:

$$
16900 + 1300 + 25 = 18225
$$

So:

$$
135^2 = 18225
$$

---

# 5. Binary Versions

The formulas above work not only in decimal but also in binary.

In decimal:

$$
B = 10
$$

In binary:

$$
B = 2
$$

Computers internally use binary, so the binary versions are very important.

---

## 5.1 Why Is Multiplication by 2^k Fast in Binary?

In decimal:

$$
123 \times 10^3 = 123000
$$

This is like appending 3 zeros in decimal.

In binary:

$$
101_2 \times 2^3 = 101000_2
$$

This is like appending 3 zeros in binary.

That means:

$$
5 \times 8 = 40
$$

because:

$$
\begin{gathered}
101_2 = 5 \\
101000_2 = 40
\end{gathered}
$$

In computers:

$$
x \times 2^k
$$

is just a left shift:

```text
x << k
```

For example:

$$
5 << 3 = 40
$$

So multiplication by `2^k` is very cheap in binary.

---

## 5.2 Binary Common-Prefix Formula

If:

$$
\begin{gathered}
A = P \times 2^k + a \\
C = P \times 2^k + c
\end{gathered}
$$

then:

$$
A \times C = P^2 \times 2^{2k} + P(a + c) \times 2^k + ac
$$

---

## 5.3 Binary Prefix Example

Let:

$$
\begin{gathered}
A = 110101_2 \\
C = 110011_2
\end{gathered}
$$

Their common prefix is:

$$
110_2
$$

The remaining 3 bits are:

$$
\begin{gathered}
101_2 \\
011_2
\end{gathered}
$$

So:

$$
\begin{gathered}
A = 110_2 \times 2^3 + 101_2 \\
C = 110_2 \times 2^3 + 011_2
\end{gathered}
$$

Convert to decimal:

$$
\begin{gathered}
110_2 = 6 \\
101_2 = 5 \\
011_2 = 3 \\
2^3 = 8
\end{gathered}
$$

So:

$$
\begin{gathered}
A = 6 \times 8 + 5 = 53 \\
C = 6 \times 8 + 3 = 51
\end{gathered}
$$

Apply the formula:

$$
\begin{gathered}
53 \times 51 \\
= 6^2 \times 2^6 + 6(5 + 3) \times 2^3 + 5 \times 3
\end{gathered}
$$

Calculate:

$$
\begin{gathered}
6^2 \times 64 = 36 \times 64 = 2304 \\
6(5 + 3) \times 8 = 6 \times 8 \times 8 = 384 \\
5 \times 3 = 15
\end{gathered}
$$

Add them:

$$
2304 + 384 + 15 = 2703
$$

Verification:

$$
53 \times 51 = 2703
$$

---

## 5.4 Binary Common-Suffix Formula

If:

$$
\begin{gathered}
A = u \times 2^k + S \\
C = v \times 2^k + S
\end{gathered}
$$

then:

$$
A \times C = uv \times 2^{2k} + (u + v)S \times 2^k + S^2
$$

Here:

```text
S
```

represents the common low-bit suffix of the two binary numbers.

---

## 5.5 Binary Suffix Example

Let:

$$
\begin{gathered}
A = 101101_2 \\
C = 111101_2
\end{gathered}
$$

Their common suffix is:

$$
1101_2
$$

So:

$$
\begin{gathered}
A = 10_2 \times 2^4 + 1101_2 \\
C = 11_2 \times 2^4 + 1101_2
\end{gathered}
$$

Convert to decimal:

$$
\begin{gathered}
10_2 = 2 \\
11_2 = 3 \\
1101_2 = 13 \\
2^4 = 16
\end{gathered}
$$

So:

$$
\begin{gathered}
A = 2 \times 16 + 13 = 45 \\
C = 3 \times 16 + 13 = 61
\end{gathered}
$$

Apply the formula:

$$
\begin{gathered}
45 \times 61 \\
= 2 \times 3 \times 2^8 + (2 + 3) \times 13 \times 2^4 + 13^2
\end{gathered}
$$

Calculate:

$$
\begin{gathered}
2 \times 3 \times 256 = 1536 \\
(2 + 3) \times 13 \times 16 = 5 \times 13 \times 16 = 1040 \\
13^2 = 169
\end{gathered}
$$

Add them:

$$
1536 + 1040 + 169 = 2745
$$

Verification:

$$
45 \times 61 = 2745
$$

---

## 5.6 Numbers Close to 2^n

If two numbers are both close to:

$$
2^n
$$

For example:

$$
\begin{gathered}
260 = 2^8 + 4 \\
270 = 2^8 + 14
\end{gathered}
$$

Then:

$$
\begin{gathered}
260 \times 270 \\
= (2^8 + 4)(2^8 + 14) \\
= 2^{16} + (4 + 14)2^8 + 4 \times 14
\end{gathered}
$$

Calculate:

$$
\begin{gathered}
2^{16} = 65536 \\
(4 + 14)2^8 = 18 \times 256 = 4608 \\
4 \times 14 = 56
\end{gathered}
$$

Add them:

$$
65536 + 4608 + 56 = 70200
$$

So:

$$
260 \times 270 = 70200
$$

This structure is useful in computers because `2^n` is represented in binary as `1` followed by many zeros.

---

# 6. Combined Prefix + Suffix Formula

Sometimes two numbers share both a common prefix and a common suffix.

For example:

$$
\begin{gathered}
A = P | x | S \\
C = P | y | S
\end{gathered}
$$

where:

| Symbol | Meaning |
|---|---|
| `P` | common prefix |
| `S` | common suffix |
| `x` | middle part of the first number |
| `y` | middle part of the second number |

If the base is `B`, the common suffix length is `s` digits, and the middle part length is `r` digits, then:

$$
\begin{gathered}
A = P \times B^{r+s} + x \times B^s + S \\
C = P \times B^{r+s} + y \times B^s + S
\end{gathered}
$$

After expansion:

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

This formula looks long, but its meaning is simple:

```text
The common prefix produces P^2
The common suffix produces S^2
The different middle parts produce xy
There are cross terms between prefix and middle
There are cross terms between prefix and suffix
There are cross terms between middle and suffix
```

This formula is suitable when:

```text
Two numbers are very similar at the front and back, with only a small middle part different.
```

If the middle different part is short, this decomposition may be useful.

---

# 7. Use Cases: When Should These Formulas Be Used?

## 7.1 When the Prefix Formula Is Suitable

When two numbers share a long front part:

```text
123456789000001
123456789000037
```

They can be written as:

$$
\begin{gathered}
P \times 10^k + a \\
P \times 10^k + c
\end{gathered}
$$

This is suitable for the common-prefix formula.

Suitable scenario:

```text
The common prefix is long, and the different suffixes are short.
```

---

## 7.2 When the Suffix Formula Is Suitable

When two numbers share a long back part:

```text
123000000000999
456000000000999
```

They can be written as:

$$
\begin{gathered}
u \times 10^k + S \\
v \times 10^k + S
\end{gathered}
$$

This is suitable for the common-suffix formula.

Suitable scenario:

```text
The common suffix is long, and the different front parts are relatively short.
```

---

## 7.3 When the Squaring Formula Is Suitable

When the two numbers are exactly the same:

$$
A \times A = A^2
$$

the squaring formula or a specialized squaring algorithm is suitable.

Squaring is faster than ordinary multiplication because:

$$
a_i \times a_j
$$

and:

$$
a_j \times a_i
$$

are repeated and can be combined into:

$$
2 \times a_i \times a_j
$$

---

## 7.4 When Binary Formulas Are Suitable

The binary versions are suitable for computers because:

$$
\times 2^k
$$

is just:

```text
left shift by k bits
```

Suitable scenarios:

```text
Numbers close to 2^n
Numbers close to 2^n - 1
Binary numbers with long common prefixes
Binary numbers with long common suffixes
```

For example:

$$
\begin{gathered}
A = 2^n + a \\
C = 2^n + c
\end{gathered}
$$

Then:

$$
A \times C = 2^{2n} + (a + c)2^n + ac
$$

If `a` and `c` are small, this is very fast.

---

## 7.5 Completely Random Large Numbers

If the numbers are completely random large numbers, these formulas cannot really be used. They are mainly useful for fast mental calculation or written calculation of structured numbers.

In that case, it is more suitable to use:

```text
ordinary multiplication
Karatsuba
Toom-Cook
NTT
FFT
```

---

# 8. Common Mistakes

## 8.1 Thinking That Sharing the First Digit Is Always Useful

For example:

$$
135 \times 186
$$

The common prefix is only:

```text
1
```

Although we can write:

$$
\begin{gathered}
135 = 1 \times 100 + 35 \\
186 = 1 \times 100 + 86
\end{gathered}
$$

and apply the formula:

$$
135 \times 186 = 1^2 \times 10000 + 1(35 + 86) \times 100 + 35 \times 86
$$

the result is correct, but we still need to calculate:

$$
35 \times 86
$$

So it is not necessarily faster.

If the common prefix is too short, the advantage is very small.

---

## 8.2 Thinking That All Cross Terms Can Be Removed

Cross terms in multiplication do not disappear. They can only be combined or processed in batches.

For example:

$$
(a + b)(c + d) = ac + ad + bc + bd
$$

The middle part:

$$
ad + bc
$$

is the cross term.

The purpose of the prefix/suffix formulas is:

```text
When some parts are identical, two cross terms can be combined into one multiplication.
```

It does not mean cross terms vanish.

---

## 8.3 Applying the Prefix Formula Without Alignment

For example:

```text
123456
12389
```

Both start with `123`, but their place values are different:

$$
\begin{gathered}
123456 = 123 \times 10^3 + 456 \\
12389  = 123 \times 10^2 + 89
\end{gathered}
$$

One is:

$$
123 \times 10^3
$$

The other is:

$$
123 \times 10^2
$$

The place values are different, so the simple common-prefix formula cannot be applied directly.

One way is to append one zero to the shorter number:

$$
12389 \times 10 = 123890
$$

Then calculate:

$$
123456 \times 123890
$$

Finally, divide the result by `10`.

But this is not necessarily faster. It is just mathematically valid.

---

## 8.4 The Suffix Must Be Consecutive from the Right

The suffix must be continuously identical from the rightmost digit.

For example:

```text
123456
789456
```

The common suffix is:

```text
456
```

so the suffix formula can be used.

But:

```text
123456
789356
```

Although some digits in the middle are the same, the consecutive common suffix from the right is only:

```text
56
```

So the common suffix can only be `56`.

---

# 9. Summary

## 9.1 Common-Prefix Formula

$$
\begin{gathered}
(P \times B^k + a)(P \times B^k + c) \\
= P^2 \times B^{2k} + P(a + c) \times B^k + ac
\end{gathered}
$$

Suitable for:

```text
Two numbers with a long common prefix.
```

---

## 9.2 Common-Suffix Formula

$$
\begin{gathered}
(u \times B^k + S)(v \times B^k + S) \\
= uv \times B^{2k} + (u + v)S \times B^k + S^2
\end{gathered}
$$

Suitable for:

```text
Two numbers with a long common suffix.
```

---

## 9.3 Squaring Formula

$$
\begin{gathered}
(P \times B^k + a)^2 \\
= P^2 \times B^{2k} + 2Pa \times B^k + a^2
\end{gathered}
$$

Suitable for:

```text
Computing the square of a number.
```

---

## 9.4 Binary Versions

Replace `B = 10` with `B = 2`.

Prefix:

$$
\begin{gathered}
(P \times 2^k + a)(P \times 2^k + c) \\
= P^2 \times 2^{2k} + P(a + c) \times 2^k + ac
\end{gathered}
$$

Suffix:

$$
\begin{gathered}
(u \times 2^k + S)(v \times 2^k + S) \\
= uv \times 2^{2k} + (u + v)S \times 2^k + S^2
\end{gathered}
$$

In computers:

```text
× 2^k = left shift by k bits
```

So binary formulas are more natural for machine calculation.

---

## 9.5 Most Important Takeaway

```text
For small numbers, these formulas are mental arithmetic tricks.
For large numbers, they are structural optimizations.
In binary, they can become shifts, additions/subtractions, and smaller multiplications.
For completely random large numbers, they usually cannot be used.
For structured large numbers, they may be very fast.
```
