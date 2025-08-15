---
title: 【manim】写一个泰勒展开式演示动画
author: Sy
cover: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
thumbnail: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
index_img: https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507151628302.png
sticky: 0
expires: ''
excerpt: 尝试三个小时用manim 速通动画制作成功
mathjax: true
tags:
  - manim
categories:
  - manim
date: 2025-07-15 16:29:00
---

> 本文将探讨如何在manim中制作泰勒展开式相关的动画...

<!-- more -->


![效果](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507151628302.png)


## 公式推导

这是泰勒展开式：
$$
f(x)=f(0)+f'(0)x+\frac{f''(0)}{2!}x^2+\dots+\frac{f^{(n)}(0)}{n!}x^n+o(x^n)
$$

欲求对$sinx$的逼近，我们将$sinx$作为$f(x)$带入公式，尝试写出几项并观察其规律：


$$
sin(x)=
\boxed{sin(0)}+
cos(0)x+
\boxed{\frac{-sin(0)}{2!}x^2}+
\frac{-cos(0)}{3!}x^3+
\boxed{\frac{sin(0)}{4!}x^4}+
\frac{cos(0)}{5!}x^5+
\boxed{\frac{-sin(0)}{6!}x^6}
$$

可以发现所有$sin(0)$全为0，约去得：
$$
sin(x)=
cos(0)x+
\frac{-cos(0)}{3!}x^3+
\frac{cos(0)}{5!}x^5
$$
可知，$cos(0)=1$：
$$
sin(x)=
\frac{x^1}{1!}+
\frac{-x^3}{3!}+
\frac{x^5}{5!}
+\dots
$$

以此类推得到：
$$
sin(x)=\sum_{i=0}^{i} 
\frac
{
	(-1)^{i} \cdot x^{2i+1}}
{(2i+1)!}
$$

转化为 python 为：

```python
def make_taylor_func(n_terms):
	return lambda x: sum(
		(-1)**k * x**(2*k + 1) / factorial(2*k + 1) for k in range(n_terms)
	)
```

于是就可以写出这样的代码：

```python
from manim import *
import numpy as np
from math import factorial

class SingleScene(Scene):
    def introduction(self):
        intro = Text("这是野生的泰勒展开式")
        tex = Tex(r"$$f(x)=f(0)+f'(0)x+\frac{f''(0)}{2!}x^2+\dots+\frac{f^{(n)}(0)}{n!}x^n+o(x^n)$$")
        intro_sin = Text("将正弦函数代入其中").shift(UP*3)
        tex_sin = Tex(r'''$$
            sin(x)=
            \boxed{sin(0)}+
            cos(0)x+
            \boxed{\frac{-sin(0)}{2!}x^2}+
            \frac{-cos(0)}{3!}x^3+
            \boxed{\frac{sin(0)}{4!}x^4}+
            \frac{cos(0)}{5!}x^5+
            \boxed{\frac{-sin(0)}{6!}x^6}
            $$
        ''').shift(DOWN).scale(0.6)
        tex_2 = Tex(r"""
            $$
            sin(x)=
            cos(0)x+
            \frac{-cos(0)}{3!}x^3+
            \frac{cos(0)}{5!}x^5
            $$
        """)
        tex_3 = Tex(r"""
            $$
            sin(x)=
            \frac{x^1}{1!}+
            \frac{-x^3}{3!}+
            \frac{x^5}{5!}
            $$
        """)
        tex_4 = Tex(r"""
            $$
            sin(x)=\sum_{i=0}^{i} 
            \frac
            {
                (-1)^{i} \cdot x^{2i+1}}
            {(2i+1)!}
            $$
        """)


        self.play(Write(intro)) # 写下 “这是野生的展开式”
        self.play(intro.animate.shift(UP * 3)) # 向上移动
        self.play(Write(tex)) # 泰勒展开式
        self.play(tex.animate.shift(UP)) # 泰勒展开式向上移动
        self.play(Unwrite(intro)) # 擦去 “这是野生的泰勒展开式”
        self.play(Write(intro_sin)) # 写下 “将正弦函数代入其中”
        self.play(Write(tex_sin)) # 写下带入正弦后的表达式
        self.play(Uncreate(tex)) # 变换得到新式子
        self.play(Unwrite(intro_sin)) # 擦去 ”带入正弦函数“
        self.play(tex_sin.animate.shift(UP)) # 将带入后的式子置于中间
        self.play(Transform(tex_sin, tex_2), run_time=2)
        self.remove(tex_sin)
        self.play(Transform(tex_2, tex_3), run_time=2)
        self.remove(tex_2)
        self.play(Transform(tex_3, tex_4), run_time=2)
        self.play(Unwrite(tex_3))

    # 创建泰勒近似函数（前 n 项）
    def make_taylor_func(self, n_terms):
        return lambda x: sum(
            (-1) ** k * x ** (2 * k + 1) / factorial(2 * k + 1) for k in range(n_terms)
        )

    # 创建对应的 LaTeX 公式
    def make_formula_tex(self, n_terms):
        terms = []
        for k in range(n_terms):
            sign = "-" if k % 2 == 1 else ""
            exponent = 2 * k + 1
            term = rf"{sign}\frac{{x^{{{exponent}}}}} {{{exponent}!}}"
            terms.append(term)
        formula = "+".join(terms).replace("+-", "-")
        return MathTex("f(x) =", formula).scale(0.8).to_edge(RIGHT)

    def write_image(self):
        ax = Axes(
            x_range=[-12, 12, 2],
            y_range=[-2, 2],
            axis_config={"color": GREEN},
        )
        pl = NumberPlane()

        sin = ax.plot(lambda x: np.sin(x), color=BLUE)

        self.play(Create(ax), Create(pl))
        self.play(Write(sin))

        # 构建图像和公式
        taylor_graphs = []
        formula_labels = []
        for i in range(1, 15):  # t1~t6
            f = self.make_taylor_func(i)
            graph = ax.plot(f, color=RED)
            formula = self.make_formula_tex(i).shift(DOWN*2)
            if (i >= 8):
                formula = formula.scale(0.5).shift(RIGHT*2)
            taylor_graphs.append(graph)
            formula_labels.append(formula)

        # 第一个图和公式
        current_graph = taylor_graphs[0]
        current_formula = formula_labels[0]

        self.play(Write(current_graph), Write(current_formula))

        # 后续逐步替换
        for next_graph, next_formula in zip(taylor_graphs[1:], formula_labels[1:]):
            self.play(Transform(current_graph, next_graph),
                      Transform(current_formula, next_formula))
            self.remove(current_graph, current_formula)
            current_graph = next_graph
            current_formula = next_formula

        # 最终版本
        self.play(Write(current_graph), Write(current_formula))
        self.wait(2)

    def construct(self):
        self.introduction()
        self.write_image()


with tempconfig({"quality": "medium_quality", "preview": True}):
    scene = SingleScene()
    scene.render()
```

下面进行逐段解释。

## introduction

该函数为画图前的公式推导铺垫动画。

```python
def introduction(self):
	intro = Text("这是野生的泰勒展开式")
	tex = Tex(r"$$f(x)=f(0)+f'(0)x+\frac{f''(0)}{2!}x^2+\dots+\frac{f^{(n)}(0)}{n!}x^n+o(x^n)$$")
	intro_sin = Text("将正弦函数代入其中").shift(UP*3)
	tex_sin = Tex(r'''$$
		sin(x)=
		\boxed{sin(0)}+
		cos(0)x+
		\boxed{\frac{-sin(0)}{2!}x^2}+
		\frac{-cos(0)}{3!}x^3+
		\boxed{\frac{sin(0)}{4!}x^4}+
		\frac{cos(0)}{5!}x^5+
		\boxed{\frac{-sin(0)}{6!}x^6}
		$$
	''').shift(DOWN).scale(0.6)
	tex_2 = Tex(r"""
		$$
		sin(x)=
		cos(0)x+
		\frac{-cos(0)}{3!}x^3+
		\frac{cos(0)}{5!}x^5
		$$
	""")
	tex_3 = Tex(r"""
		$$
		sin(x)=
		\frac{x^1}{1!}+
		\frac{-x^3}{3!}+
		\frac{x^5}{5!}
		$$
	""")
	tex_4 = Tex(r"""
		$$
		sin(x)=\sum_{i=0}^{i} 
		\frac
		{
			(-1)^{i} \cdot x^{2i+1}}
		{(2i+1)!}
		$$
	""")


	self.play(Write(intro)) # 写下 “这是野生的展开式”
	self.play(intro.animate.shift(UP * 3)) # 向上移动
	self.play(Write(tex)) # 泰勒展开式
	self.play(tex.animate.shift(UP)) # 泰勒展开式向上移动
	self.play(Unwrite(intro)) # 擦去 “这是野生的泰勒展开式”
	self.play(Write(intro_sin)) # 写下 “将正弦函数代入其中”
	self.play(Write(tex_sin)) # 写下带入正弦后的表达式
	self.play(Uncreate(tex)) # 变换得到新式子
	self.play(Unwrite(intro_sin)) # 擦去 ”带入正弦函数“
	self.play(tex_sin.animate.shift(UP)) # 将带入后的式子置于中间
	self.play(Transform(tex_sin, tex_2), run_time=2)
	self.remove(tex_sin)
	self.play(Transform(tex_2, tex_3), run_time=2)
	self.remove(tex_2)
	self.play(Transform(tex_3, tex_4), run_time=2)
	self.play(Unwrite(tex_3))
```

 较为简单，用到`Animations`和`Tex`。

## write_image

此程序的难点所在。

先来看两个工具函数：
```python
# 创建泰勒近似函数（前 n 项）
def make_taylor_func(self, n_terms):
	return lambda x: sum(
		(-1) ** k * x ** (2 * k + 1) / factorial(2 * k + 1) for k in range(n_terms)
	)

# 创建对应的 LaTeX 公式
def make_formula_tex(self, n_terms):
		terms = []
		for k in range(n_terms):
				sign = "-" if k % 2 == 1 else ""
				exponent = 2 * k + 1
				term = rf"{sign}\frac{{x^{{{exponent}}}}} {{{exponent}!}}"
				terms.append(term)
		formula = "+".join(terms).replace("+-", "-")
		return MathTex("f(x) =", formula).scale(0.8).to_edge(RIGHT)
```

`make_taylor_func`用于求每一个子项的大小，相当于：
$$
\sum_{i=0}^{i} 
\frac
{
	(-1)^{i} \cdot x^{2i+1}}
{(2i+1)!}
$$
通过`for`循环生成了`k`项，也就是这里的$i$。

`make_formula_tex`用于产生每个展开式对应的`Latex`对象。先用正则匹配生成`k`条字符串，放入数组并拼接起来得到最终的公式。需要注意部分项会产生`-`，拼接后就变成`+-`，需统一替换成`-`。

最终`write_image(self)`如下，我们逐段研究。

```python
def write_image(self):
	ax = Axes(
		x_range=[-12, 12, 2],
		y_range=[-2, 2],
		axis_config={"color": GREEN},
	)
	pl = NumberPlane()

	sin = ax.plot(lambda x: np.sin(x), color=BLUE)

	self.play(Create(ax), Create(pl))
	self.play(Write(sin))

	# 构建图像和公式
	taylor_graphs = []
	formula_labels = []
	for i in range(1, 15):  # t1~t6
		f = self.make_taylor_func(i)
		graph = ax.plot(f, color=RED)
		formula = self.make_formula_tex(i).shift(DOWN*2)
		if (i >= 8):
			formula = formula.scale(0.5).shift(RIGHT*2)
		taylor_graphs.append(graph)
		formula_labels.append(formula)

	# 第一个图和公式
	current_graph = taylor_graphs[0]
	current_formula = formula_labels[0]

	self.play(Write(current_graph), Write(current_formula))

	# 后续逐步替换
	for next_graph, next_formula in zip(taylor_graphs[1:], formula_labels[1:]):
		self.play(Transform(current_graph, next_graph),
				  Transform(current_formula, next_formula))
		self.remove(current_graph, current_formula)
		current_graph = next_graph
		current_formula = next_formula

	# 最终版本
	self.play(Write(current_graph), Write(current_formula))
	self.wait(2)
```

### section 1

第一部分建立坐标系，并画图，难度不大。

```python
ax = Axes(
		x_range=[-12, 12, 2],
		y_range=[-2, 2],
		axis_config={"color": GREEN},
	)
	pl = NumberPlane()
	
	sin = ax.plot(lambda x: np.sin(x), color=BLUE)
	
	self.play(Create(ax), Create(pl))
	self.play(Write(sin))
```

### section 2

第二部分先是建立两个数组用于存放未来要画的泰勒函数图像，公式标签在右下角展示使视频更直观。通过循环生成了 15 个表达式，f 为我们根据`make_taylor_func`建立的函数对象。将函数对象 f 放入`ax.plot()`中即可得到图像对象，公式`formula`根据`make_formula_tex()`处理得到。

> 由于公式较长时会溢出屏幕，需在 $i\geq8$ 时缩小其尺寸。   

```python
 # 构建图像和公式
taylor_graphs = []
formula_labels = []
for i in range(1, 15):  # t1~t6
	f = self.make_taylor_func(i)
	graph = ax.plot(f, color=RED)
	formula = self.make_formula_tex(i).shift(DOWN*2)
	if (i >= 8):
		formula = formula.scale(0.5).shift(RIGHT*2)
	taylor_graphs.append(graph)
	formula_labels.append(formula)
```

### section 3

先画出第一个展开式的图像，后续展开式则用数组和循环批量处理。后续通过切片语法从第二个展开式开始迭代。每一次更新都播放衔接的过渡动画，移除当前处理的公式、函数图像。并将`current_xxx`指针指向下一个带处理的对象。留下最后一组时，上一组对象已被循环移除，直接播放动画即可。

```python
# 第一个图和公式
current_graph = taylor_graphs[0]
current_formula = formula_labels[0]

self.play(Write(current_graph), Write(current_formula))

# 后续逐步替换
for next_graph, next_formula in zip(taylor_graphs[1:], formula_labels[1:]):
	self.play(Transform(current_graph, next_graph),
			  Transform(current_formula, next_formula))
	self.remove(current_graph, current_formula)
	current_graph = next_graph
	current_formula = next_formula

# 最终版本
self.play(Write(current_graph), Write(current_formula))
self.wait(2)
```

> 这里的`zip`函数用于将两个数组并排循环，本质仍在对数组进行迭代，例如：


```python
a = [1, 2, 3]
b = ['a', 'b', 'c']

for x, y in zip(a, b):
    print(x, y)

# 输出:
# 1 a
# 2 b
# 3 c
```
