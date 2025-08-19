---
title: 【Latex】 LaTeX 备忘录
author: Sy
cover: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
thumbnail: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
sticky: 0
expires: ''
excerpt: 其他...
mathjax: true
index_img: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507150143176.png'
tags:
  - 其他
category: 其他
categories:
  - 其他
date: 2025-07-15 01:41:00
---

## 常用符号

| 代码                             | 效果                           | 说明             | 简码                 |
| ------------------------------ | ---------------------------- | -------------- | ------------------ |
| `$$ \sum_{i=0}^{n}i^2 $$`      | $$\sum_{i=0}^{n}i^2$$        | 求和             | `\sum`             |
| `$$ \prod_{i=1}^n $$`          | $$\prod_{i=1}^n$$            | 累乘             | `\prod`            |
| `$$ \lim_{x\to0}x $$`          | $$\lim_{x\to0}x$$            | 极限             | `\lim`             |
| `$$ \int_a^b x dx $$`          | $$\int_a^b x dx$$            | 积分             | `\int`             |
| `$\iiint$`                     | $\iiint$                     | 多重积分，$i$的个数为重数 | `\iiint`           |
| `$\idotsint$`                  | $\idotsint$                  | 带省略的积分号        | `\idotsint`        |
| `$ \boxed{E=mc^2} $`           | $$ \boxed{E=mc^2} $$         | 加框             | `\boxed{}`         |
| `$\to$`                        | $\to$                        | 上面极限中的箭头符号     | `\to`              |
| `$\leftarrow$`                 | $\leftarrow$                 | 左箭头            | `\leftarrow`       |
| `$\rightarrow$`                | $\rightarrow$                | 右箭头            | `\rightarrow`      |
| `$\Leftrightarrow$`            | $\Leftrightarrow$            | 左右箭头，充要符号      | `\Leftrightarrow`  |
| `$$\xrightarrow[x<y]{x+y+z}$$` | $$\xrightarrow[x<y]{x+y+z}$$ | 带有说明的右箭头       | `\xrightarrow[]{}` |
| `$$\xleftarrow[x<y]{x+y+z}$$`  | $$\xleftarrow[x<y]{x+y+z}$$  | 带有说明的左箭头       | `\xleftarrow[]{}`  |
| `$\xlongequal{\text{{条件}}}$`   | $\xlongequal{\text{{条件}}}$   | 带有条件的等号        | `\xlongequal{}`    |
| `$x^2$`                        | $x^2$                        | 上标             | `^`                |
| `$x_1$`                        | $x_1$                        | 下标             | `_`                |
| `$\sqrt[n]{x^2}$`              | $\sqrt[n]{x^2}$              | 根号             | `\sqrt[n]`         |
| `$\quad$`                      | $\quad$                      | 空格             | `\quad`            |
| `$\frac{a}{b}$`                | $\frac{a}{b}$                | 分数             | `\frac{}{}`        |
| `$\pm$`                        | $\pm$                        | 正负号            | `\pm`              |
| `$\times$`                     | $\times$                     | 乘              | `\times`           |
| `$\div$`                       | $\div$                       | 除              | `\div`             |
| `$\cdot$`                      | $\cdot$                      | 点乘             | `\cdot`            |
| `$\cap$`                       | $\cap$                       | 与              | `\cap`             |
| `$\cup$`                       | $\cup$                       | 或              | `\cup`             |
| `$\geq$`                       | $\geq$                       | 大于等于           | `\geq`             |
| `$\leq$`                       | $\leq$                       | 小于等于           | `\leq`             |
| `$\neq$`                       | $\neq$                       | 不等于            | `\neq`             |
| `$\approx$`                    | $\approx$                    | 约等于            | `\approx`          |
| `$\equiv$`                     | $\equiv$                     | 全等于            | `\equiv`           |

对于求和与累乘等，本质上只是和数字单个符号，其求和通过上下标语法来表示。求和类型符号在行内会被压缩，在`$$`情况下会正常显示。

<!-- more -->
## 分支公式（分段函数）

$$
y=\begin{cases}
-x, \quad x \leq 0 \\
x, \quad x > 0
\end{cases}
$$
```latex
$$
y=\begin{cases}
-x, \quad x \leq 0 \\
x, \quad x > 0
\end{cases}
$$
```

## 长公式

$$
\begin{multline}
x=a+b+c+{} \\
d+e+f
\end{multline}
$$
```latex
$$
\begin{multline}
x=a+b+c+{} \\
d+e+f
\end{multline}
$$
```

$$
\begin{split} x = {} & a + b + c +{}\\ &d + e + f + g \end{split}
$$

```latex
\begin{split} x = {} & a + b + c +{}\\ &d + e + f + g \end{split}
```

## 公式组
$$
\begin{gather}
a = b+c+d\\
x=y+z
\end{gather}
$$

```latex
\begin{gather}
a = b+c+d\\
x=y+z
\end{gather}
```

$$
\begin{align}
a &=b+c+d \\
x &=y+z
\end{align}
$$

```latex
$$
\begin{align}
a &=b+c+d \\
x &=y+z
\end{align}
$$
```

## 矩阵

$$
\begin{pmatrix} a & b\\ c & d \\ \end{pmatrix} \quad
\begin{bmatrix} a & b \\ c & d \\ \end{bmatrix}\quad
\begin{Bmatrix} a & b \\ c & d\\ \end{Bmatrix}\quad
\begin{vmatrix} a & b \\ c & d \\ \end{vmatrix}\quad
\begin{Vmatrix} a & b\\ c & d \\ \end{Vmatrix}
$$

```latex
$$
\begin{pmatrix} a & b\\ c & d \\ \end{pmatrix} \quad
\begin{bmatrix} a & b \\ c & d \\ \end{bmatrix}\quad
\begin{Bmatrix} a & b \\ c & d\\ \end{Bmatrix}\quad
\begin{vmatrix} a & b \\ c & d \\ \end{vmatrix}\quad
\begin{Vmatrix} a & b\\ c & d \\ \end{Vmatrix}
$$
```

## 省略号
$$\dots \quad \cdots \quad \vdots \quad \ddots$$

```latex
$$\dots \quad \cdots \quad \vdots \quad \ddots$$
```

## 常用希腊字母标

| 字母            | 代码            | 字母          | 代码          | 字母          | 代码          | 字母         | 代码         |
| ------------- | ------------- | ----------- | ----------- | ----------- | ----------- | ---------- | ---------- |
| $\alpha$      | `\alpha`      | $\theta$    | `\theta`    | $o$         | `o`         | $\tau$     | `\tau`     |
| $\beta$       | `\beta`       | $\vartheta$ | `\vartheta` | $\pi$       | `\pi`       | $\upsilon$ | `\upsilon` |
| $\gamma$      | `\gamma`      | $\iota$     | `\iota`     | $\varpi$    | `\varpi`    | $\phi$     | `\phi`     |
| $\delta$      | `\delta`      | $\kappa$    | `\kappa`    | $\rho$      | `\rho`      | $\varphi$  | `\varphi`  |
| $\epsilon$    | `\epsilon`    | $\lambda$   | `\lambda`   | $\varrho$   | `\varrho`   | $\chi$     | `\chi`     |
| $\varepsilon$ | `\varepsilon` | $\mu$       | `\mu`       | $\sigma$    | `\sigma`    | $\psi$     | `\psi`     |
| $\zeta$       | `\zeta`       | $\nu$       | `\nu`       | $\varsigma$ | `\varsigma` | $\omega$   | `\omega`   |
| $\eta$        | `\eta`        | $\xi$       | `\xi`       | $\Sigma$    | `\Sigma`    | $\Psi$     | `\Psi`     |
| $\Gamma$      | `\Gamma`      | $\Lambda$   | `\Lambda`   | $\Upsilon$  | `\Upsilon`  | $\Omega$   | `\Omega`   |
| $\Delta$      | `\Delta`      | $\Xi$       | `\Xi`       | $\Phi$      | `\Phi`      |            |            |
| $\Theta$      | `\Theta`      | $\Pi$       | `\Pi`       |             |             |            |            |
## 自定义命令

在 `main.tex` 中加入 `\input{commandfile}`, commandfile 为存放命令的文件.

在 commandfile 中添加命令集合即可:

1. Newcommand 替代常出现的文本
```latex
\newcommand{\tnss}{The not so Short Introduction to \LaTeX}
```

2. 替代文本且带有参数

```latex
\newcommand{\txist}[1] {This is the \emph{#1} Short Introduction to \LaTeX} \txist{not so}\\ \txist{very}
```

3. Def 简化命令

```latex
\def\E{\mathrm{E}} \def\Var{\mathrm{Var}} \def\Cov{\mathrm{Cov}}
```