---
title: 【manim】Axes 坐标系及其基类详解
author: Sy
index_img: https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2013.13.48.png
cover: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
thumbnail: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
sticky: 0
expires: ''
excerpt: Axes 坐标系使用方法介绍
date: 2025-07-11 16:26:15
categories:
    - manim
tags: manim
---

在 `Manim` 中制作与参考系相关的数学动画，首先要了解 manim 中数学坐标系相关的 Mobject 用法，其次还牵涉到其余的个别 Mobject 的使用。学会了 Mobject 还不够，你得让坐标系动起来吧？那就要会使用 Animations 类，最好通读一遍文档。完成了这些还不够，Animations 类没法制作动态跟踪的动画，如果需要让某个在坐标上移动，并且同步显示其位置还需要学会 mainm 中 valueTracker 的使用。

<!-- more -->
  

关于 Animations 在我的一篇文章中有过介绍，不详细介绍了。本文主要来探讨一下坐标系（Axes）及 ValueTracker 的搭配使用，从零开始构建一个数学参考系的函数动画演示。

  

下文中为了方便尽量使用`self.add()`方法。本质上`Animations`类中的方法如出一辙，在学习阶段并不是说一定要用到。`self.add()`已经足够快捷方便了。

  

> 本文中所有内容均完全基于 **manim community** 参考手册，独立原创。

  

![Manim Title](https://docs.manim.community/en/stable/_static/manim-logo-sidebar.svg)

  

## 坐标轴 Axes

  

一个数学动画由哪些元素构成？首先得有平面坐标系吧，坐标系肯定有各种图例和标签符号，这些都是 Mobject。我们先来研究研究坐标系的详细使用方法，再去探讨函数图像的绘制，最后我们讨论一些特殊的 Mobject 怎么用（例如箭头、数字等），在文章末尾我们将他们串连在一起，用 valueTracker 实现动态的渲染。

  

要了解坐标系，最好的办法当然是查阅一手资料 -- [官方文档](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.html)。就在手册的 **Reference Manual > Mobjects > graphing > coordinate_systems** 下，我们主要来看 Axes 和 Coordinate System 怎么用。

  

### 构造方法

  

![参考文档](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2011.58.35.png)

先来看看 Axes 类的构造方法：

```python

class Axes(x_range=None, y_range=None, x_length=12, y_length=6, axis_config=None, x_axis_config=None, y_axis_config=None, tips=True, **kwargs)

```

  

我们大致可以看出，Axes 在构造时可以设置 $x$ 轴与 $y$ 轴的长度、取值范围、传递参数、提示等信息。

  

![官方参数说明](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.07.31.png)

  

这是一段朴实无华的 Axes 构造生成的轴：

  

![朴实无华且单调，但不是递增...](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/SingleScene_ManimCE_v0.19.0_%E5%89%AF%E6%9C%AC.png)

  

```python
from manim import *
class SingleScene(Scene):
	def construct(self):
		axe = Axes()
		self.add(axe)
```

  

我们给他加上取值范围：

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507150010753.png)


```python

from manim import *
class SingleScene(Scene):
	def construct(self):
		axe = Axes(
			x_range=[1, 10, 1],
			y_range=[-1, 10, 2]
		)
		self.add(axe)
```

  

我们令`tips=False`，发现箭头没了：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507150011806.png)

  

```python
from manim import *
class SingleScene(Scene):
    def construct(self):
        axe = Axes(x_range=[1, 10, 1], y_range=[-1, 10, 2], tips=False)
        self.add(axe)
```

  

加上配置项`axis_config={"include_numbers": True}`便有了数字：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.34.58.png)

```python

from manim import *
  
class SingleScene(Scene):
	def construct(self):
	axe = Axes(
		x_range=[1, 10, 1],
		y_range=[-1, 10, 2],
		axis_config={"include_numbers": True},
		tips=False
	)
	self.add(axe)

```

  

加上`y_axis_config={"scaling": LogBase(custom_labels=True)},`会发现y 坐标有了科学计数法：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.37.10.png)

```python

from manim import *
class SingleScene(Scene):
	def construct(self):
		axe = Axes(
			x_range=[1, 10, 1],
			y_range=[-1, 10, 2],
			axis_config={"include_numbers": True},
			tips=False,
			y_axis_config={"scaling": LogBase(custom_labels=True)},
		)
		self.add(axe)

```

  

需要注意的是，这里启动了 y 轴的对数刻度显示，也就是在轴上的任何函数都是基于对数的形式绘制的。

我们修改一下范围，就会得到官方文档中的样式：


![对数图像](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.44.15.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		axe = Axes(
			x_range=[0, 10, 1],
			y_range=[-2, 6, 1],
			axis_config={"include_numbers": True},
			y_axis_config={"scaling": LogBase(custom_labels=True)},
			tips=False,
		)
		graph = axe.plot(lambda x: x ** 2, x_range=[0.001, 10], use_smoothing=False)
		self.add(axe, graph)

```

  

尽管这里的 lambda 表达式中写了 `x ** 2`，但函数实际上绘制了$log_x^2$。

我们把`axis_config`的配置项改为`{"include_numbers": True, 'tip_shape': StealthTip},`会发现轴线的指示箭头改变了。

![箭头底部向上凹](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.47.34.png)

  

接下来我们稍微快一点。我们可以在图中加入一个`NumberPlane`对象来产生网格坐标背景，并将绘制出的函数颜色改为红色使之更加鲜明。

![开始大刀阔斧地改造](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2012.52.35.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		plane = NumberPlane()
		axe = Axes(
			x_range=[0, 10, 1],
			y_range=[-2, 6, 1],
			axis_config={"include_numbers": True, 'tip_shape': StealthTip},
			y_axis_config={"scaling": LogBase(custom_labels=True)},
		)
		graph = axe.plot(lambda x: x ** 2, x_range=[0.001, 10], use_smoothing=False, color=RED)
		self.add(axe, graph, plane)

```

### 类方法

  

我们通过一个例子来讲解一下几个类方法的作用：


![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2013.01.11.png)

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		plane = NumberPlane()
		axe = Axes().add_coordinates()
		dot = Dot((2, 2, 0), color=GREEN)
		dot2 = Dot(axe.coords_to_point(2, 2), color=WHITE)
		graph = axe.plot(lambda x: x ** 2, x_range=[0.001, 10], use_smoothing=False, color=RED)
		self.add(axe, graph, plane, dot, dot2)

```

  

在这段代码中，用 `add_coordinates()` 为白色的十字数轴添加了数字的坐标轴指示，并创建了两个 `Dot` 点对象。你会明显看到两个点的坐标都是`(2,2)`但其位置不同。

  

直接根据点的构造方法创建的绿点，其位置的`(2,2)`是相对于`plane`而言的，也就是整个平面坐标系 coordinates。通过`coords_to_point`就可以将默认的`coordinate`坐标转换成数轴上的`(2,2)`坐标，也就是白点实现的效果。

  

另外，我们还删除了原有`Axe`中的配置项参数，如果自定义范围会导致数轴不在参考系的中间，或者说不在屏幕中心。

  

我们通过`get_lines_to_point`来获得一个到达目标目标点的虚线（我们常常会画的辅助线），这里获取点的坐标并没有直接采用`dot`，而是通过`axe.c2p(2,2)`来得到一个 “coordinate_to_point” 的点，注意这里为缩写。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2013.09.04.png)

  

```python

from manim import *
class SingleScene(Scene):
	def construct(self):
		plane = NumberPlane()
		axe = Axes().add_coordinates()
		dot = Dot((2, 2, 0), color=GREEN)
		dot2 = Dot(axe.coords_to_point(2, 2), color=WHITE)
		lines = axe.get_lines_to_point(axe.c2p(2,2))
		graph = axe.plot(lambda x: x ** 2, x_range=[0.001, 10], use_smoothing=False, color=RED)
		self.add(axe, graph, plane, dot, dot2, lines)
```

  

我们来为$x$和$y$轴加上他们的标签：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2013.13.48.png)

利用`axe.get_x_axis_label()`我们可以直接利用`axe`得到一个确定好位置的轴线坐标，他是一个 label 对象，避免了单独创建一个 label 并调整位置。

  

> 同理，也可以使用官网中的`get_axis_labels()`方法，传入两个`Tex`对象即可。

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		plane = NumberPlane()
		axe = Axes().add_coordinates()
		dot = Dot((2, 2, 0), color=GREEN)
		dot2 = Dot(axe.coords_to_point(2, 2), color=WHITE)
		lines = axe.get_lines_to_point(axe.c2p(2,2))
		graph = axe.plot(lambda x: x ** 2, x_range=[0.001, 10], use_smoothing=False, color=RED)
		x_label = axe.get_x_axis_label(Tex('x'))
		y_label = axe.get_y_axis_label(Tex('y').scale(2))
		self.add(axe, graph, plane, dot, dot2, lines, x_label, y_label)
```

  

我们来画一个圆，并显示其位置。圆心向上平移两个单位，白点为圆最右端点。`np.around`用于保留小数。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2013.40.05.png)

  

```python

from manim import *
class SingleScene(Scene):
	def construct(self):
		plane = NumberPlane()
		axe = Axes(x_range=[0,10,2]).add_coordinates()
		circ = Circle().shift(UP*2)
		coords = np.around(axe.point_to_coords(circ.get_right()), decimals=2) # 得到 circ 右边的点并转化成 coords 坐标，保留两位小数
		label = (Matrix([[coords[0]], [coords[1]]]).next_to(circ, RIGHT)) # 在圆的右边放一个矩阵
		self.add(axe, circ, Dot(circ.get_right()), plane, label)

```

## 坐标系 CoordinateSystem

**CoordinateSystem** 是 **Axes** 的抽象基类。

文档中一上来就给出了一个相对复杂的例子，我自己也实现了一遍，实际上还是用的 Axes 中的方法：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2014.19.40.png)

  

```python

from manim import *
class SingleScene(Scene):
	def construct(self):
		axe = Axes(
			x_range=[0,1,0.05],
			y_range=[0,1,0.05],
			x_length=10,
			y_length=5,
			axis_config={
			"numbers_to_include": np.arange(0, 1, 0.1),
			"font_size": 24
			},
			tips=False
		)
		
		label_x = axe.get_x_axis_label("x")
		label_y = axe.get_y_axis_label("y", direction=LEFT, edge=LEFT, buff=0.4)
		labels = VGroup()
		labels.add(label_x)
		labels.add(label_y)
		lines = VGroup()
		for i in np.arange(1, 20+0.5, 0.5):
		labels.add(axe.plot(lambda x: x**i, color=BLUE))
		labels.add(axe.plot(lambda x: x**(1/i), color=PINK))
		
		dot = Dot(axe.c2p(1, 1), color=YELLOW)
		p_line = axe.get_lines_to_point(axe.coords_to_point(1,1))
		title = Title(
			# spaces between braces to prevent SyntaxError
			r"Graphs of $y=x^{ {1}\over{n} }$ and $y=x^n (n=1,2,3,...,20)$",
			include_underline=False,
			font_size=40,
		)
		self.add(axe, lines, labels, dot, p_line, title)

```

  

仍然是使用 `Axes` 来代表坐标系，用到了几个经常出现的方法和函数。`VGroup`用于逻辑上将几个 Mobjects 放在一起处理，不影响其本身的属性。用`c2p`来生成数轴的坐标点，`coords_to_point`是其全拼本质上一致。

  

他提供的方法较多：

  

| 方法名 | 作用 |
|---|---|
| [`add_coordinates`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.add_coordinates "manim.mobject.graphing.coordinate_systems.CoordinateSystem.add_coordinates") | 向轴添加标签。 |
| [`angle_of_tangent`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.angle_of_tangent "manim.mobject.graphing.坐标系统.坐标系统.切线角") | 返回在特定 x 值处绘制曲线的切线与 x 轴的夹角。 |
| [`c2p`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.c2p "manim.mobject.graphing.coordinate_systems.CoordinateSystem.c2p") | 缩写`coords_to_point()` |
| `coords_to_point` | |
| [`get_T_label`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_T_label "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_T_label") | 创建一个带标签的三角形标记，其中有一条垂直线从 x 轴到给定 x 值的曲线。 |
| [`get_area`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_area "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_area") | 返回[`Polygon`](https://docs.manim.community/en/stable/reference/manim.mobject.geometry.polygram.Polygon.html#manim.mobject.geometry.polygram.Polygon "manim.mobject.geometry.polygram.多边形")表示所传递的图表下方的区域。 |
| `get_axes` | |
| `get_axis` | |
| `get_axis_labels` | |
| [`get_graph_label`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_graph_label "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_graph_label") | 为传递的图形创建一个正确定位的标签，带有可选的点。 |
| [`get_horizontal_line`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_horizontal_line "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_horizo​​ntal_line") | 从 y 轴到场景中给定点的水平线。 |
| [`get_line_from_axis_to_point`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_line_from_axis_to_point "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_line_from_axis_to_point") | 返回从给定轴到场景中某个点的直线。 |
| [`get_lines_to_point`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_lines_to_point "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_lines_to_point") | 生成从轴到某个点的水平线和垂直线。 |
| [`get_origin`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_origin "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_origin") | 获取的起源[`Axes`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.Axes.html#manim.mobject.graphing.coordinate_systems.Axes "manim.mobject.graphing.坐标系统.轴")。 |
| [`get_riemann_rectangles`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_riemann_rectangles "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_riemann_rectangles") | [`VGroup`](https://docs.manim.community/en/stable/reference/manim.mobject.types.vectorized_mobject.VGroup.html#manim.mobject.types.vectorized_mobject.VGroup "manim.mobject.types.vectorized_mobject.VGroup")为给定曲线生成黎曼矩形。 |
| [`get_secant_slope_group`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_secant_slope_group "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_secant_slope_group") | 创建两条线，分别表示dx和df ，即dx和df的标签，以及 |
| [`get_vertical_line`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_vertical_line "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_vertical_line") | 从 x 轴到场景中给定点的垂直线。 |
| [`get_vertical_lines_to_graph`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_vertical_lines_to_graph "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_vertical_lines_to_graph") | 获取从 x 轴到曲线的多条线。 |
| `get_x_axis` | |
| [`get_x_axis_label`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_x_axis_label "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_x_axis_label") | 生成 x 轴标签。 |
| `get_x_unit_size` | |
| `get_y_axis` | |
| [`get_y_axis_label`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_y_axis_label "manim.mobject.graphing.coordinate_systems.CoordinateSystem.get_y_axis_label") | 生成 y 轴标签。 |
| `get_y_unit_size` | |
| `get_z_axis` | |
| [`i2gc`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.i2gc "manim.mobject.graphing.coordinate_systems.CoordinateSystem.i2gc") | 的别名[`input_to_graph_coords()`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_coords "manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_coords")。 |
| [`i2gp`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.i2gp "manim.mobject.graphing.coordinate_systems.CoordinateSystem.i2gp") | 的别名[`input_to_graph_point()`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_point "manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_point")。 |
| [`input_to_graph_coords`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_coords "manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_coords") | 根据给定的 x 值返回图表上点的轴相对坐标的元组。 |
| [`input_to_graph_point`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_point "manim.mobject.graphing.coordinate_systems.CoordinateSystem.input_to_graph_point") | `graph`返回对应于某个值的点的坐标`x`。 |
| [`p2c`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.p2c "manim.mobject.graphing.coordinate_systems.CoordinateSystem.p2c") | 缩写`point_to_coords()` |
| [`plot`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot") | 根据函数生成曲线。 |
| [`plot_antiderivative_graph`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_antiderivative_graph "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_antiderivative_graph") | 绘制不定积分图。 |
| [`plot_derivative_graph`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_derivative_graph "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_derivative_graph") | 返回传递的图形的导数曲线。 |
| [`plot_implicit_curve`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_implicit_curve "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_implicit_curve") | 创建隐函数的曲线。 |
| [`plot_parametric_curve`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_parametric_curve "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_parametric_curve") | 参数曲线。 |
| [`plot_polar_graph`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_polar_graph "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_polar_graph") | 极坐标图。 |
| [`plot_surface`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_surface "manim.mobject.graphing.coordinate_systems.CoordinateSystem.plot_surface") | 根据函数生成表面。 |
| `point_to_coords` | |
| [`point_to_polar`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.point_to_polar "manim.mobject.graphing.坐标系统.坐标系统.点到极坐标") | 从一个点获取极坐标。 |
| [`polar_to_point`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.polar_to_point "manim.mobject.graphing.坐标系统.坐标系统.极地到点") | 从极坐标获取一个点。 |
| [`pr2pt`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.pr2pt "manim.mobject.graphing.coordinate_systems.CoordinateSystem.pr2pt") | 缩写[`polar_to_point()`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.polar_to_point "manim.mobject.graphing.坐标系统.坐标系统.极地到点") |
| [`pt2pr`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.pt2pr "manim.mobject.graphing.coordinate_systems.CoordinateSystem.pt2pr") | 缩写[`point_to_polar()`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.point_to_polar "manim.mobject.graphing.坐标系统.坐标系统.点到极坐标") |
| [`slope_of_tangent`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.coordinate_systems.CoordinateSystem.html#manim.mobject.graphing.coordinate_systems.CoordinateSystem.slope_of_tangent "manim.mobject.graphing.coordinate_systems.CoordinateSystem.切线斜率") | 返回特定 x 值处绘制曲线的切线斜率。 |


我们着重挑几个比较有意思的玩玩。

  

### CoordinateSystem.get_T_label()

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2014.33.26.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		# defines the axes and linear function
		axes = Axes(x_range=[-10, 10], y_range=[-1, 10], x_length=9, y_length=6).add_coordinates()
		func = axes.plot(lambda x: x*x, color=BLUE)
		# creates the T_label
		t_label = axes.get_T_label(x_val=4, graph=func, label=Tex("x-value"))
		self.add(axes, func, t_label)
```

  

tLabel 就是图中黄色的线+xvalue 的标签组合，只需要指定一个`x`坐标，并选择一条函数曲线即可。

  

### CoordinateSystem.get_area()

  

可以获取函数和坐标中之间的面积。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2014.38.27.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		# defines the axes and linear function
		axes = Axes(x_range=[-10, 10], y_range=[-1, 10], x_length=9, y_length=6).add_coordinates()
		func = axes.plot(lambda x: np.sin(x*PI/2), color=BLUE)
		# creates the T_label
		t_label = axes.get_T_label(x_val=5, graph=func, label=Tex("x-value"))
		area = axes.get_area(
			func,
		)
		self.add(axes, func, t_label, area)

```

  

还可以修改其颜色、透明度、范围来达到这样的效果：

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/getarea.png)

  

```python

area = axes.get_area(
	func,
	opacity=.4,
	color=GREEN,
	x_range=(-1, 2)
)
```

  

### CoordinateSystem.get_graph_label()

  

这个方法用于获取坐标系中的图例，比如修改前面 **get_area** 中的代码，增加一个 **label**。在这里我们这只为了带点的标签，通过 **label** 属性设置其内容，**graph** 设置其作用的函数，用 **x_val** 指定了在函数上的位置，**direction** 为偏移方向，以及颜色等等。

  
  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2014.46.37.png)

  

```python

label = axes.get_graph_label(
	graph=func,
	label=MathTex(r"1.6"),
	dot=True,
	x_val=1.6,
	direction=UP,
	color=YELLOW
)

self.add(axes, func, t_label, area, label)

```

  
  

### CoordinateSystem.get_horizontal_line()

  

得到一条从点到$y$轴的水平线，对应的是 `get_vertical_line()`。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2014.56.57.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		# defines the axes and linear function
		axes = Axes(x_range=[-10, 10], y_range=[-1, 10], x_length=9, y_length=6).add_coordinates()
		func = axes.plot(lambda x: np.sin(x*PI/2), color=BLUE)
		# creates the T_label
		point = axes @ (2, 1)
		dot = Dot(point)
		line = axes.get_horizontal_line(point, line_func=Line)
		self.add(axes, func, dot, line)

```

  

注意这里的第一个参数是`point`不是 dot 对象。`@`表示 python 中的矩阵运算。

  

传入参数`line_config={"dashed_ratio": 0.85}`设置虚线并调整图线比例。

  

### CoordinateSystem.get_lines_to_point()

  

同时得到垂直和水平的垂线。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2015.05.07.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		# defines the axes and linear function
		axes = Axes()
		circ = Circle(color=DARK_BLUE).shift(DL*2)
		point = Dot(circ.get_right())
		right_line = axes.get_lines_to_point(circ.get_right())
		corner_line = axes.get_lines_to_point(circ.get_corner(DL))
		self.add(axes, circ, point, right_line, corner_line)

```

  

只需提供点的坐标即可。

  

### CoordinateSystem.get_riemann_rectangles()

  

为曲线生成黎曼矩形。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2015.50.30.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		# defines the axes and linear function
		axes = Axes().add_coordinates()
		func = axes.plot(lambda x: x**2)
		rects = axes.get_riemann_rectangles(
			func,
			dx=0.25,
			input_sample_type="right",
			x_range=[-3, -1]
		)
		self.add(axes, func, rects)

```

  

`input_sample_type`用于设置黎曼矩形和曲线接触的点为右上角端点，默认为左上角。`dx`设置矩形的宽度，`dx`越大，就越大越稀疏。

  
  

### CoordinateSystem.get_secant_slope_group()

  

获得切线组，也就是包括割线在内一整个三角形组。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2015.58.35.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		ax = Axes(y_range=[-1, 7])
		graph = ax.plot(lambda x: 1 / 4 * x ** 2, color=BLUE)
		slopes = ax.get_secant_slope_group(
			x=2.0,
			graph=graph,
			dx=2,
			dx_label=Tex("dx = 1.0"),
			dy_label="dy",
			dx_line_color=GREEN_B,
			secant_line_length=4,
			secant_line_color=RED_D,
		)
		self.add(ax, graph, slopes)
```

  
  
  

可以根据配置项来确定 x 的起点，水平距离，割线长度等等信息。

  

### CoordinateSystem.get_vertical_line()

  

获取从 x 轴到曲线的多条线。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2016.03.57.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		ax = Axes(y_range=[-1, 7])
		graph = ax.plot(lambda x: 1 / 4 * x ** 2, color=BLUE)
		slopes = ax.get_vertical_lines_to_graph(
			graph,
			x_range=[-2, 4],
			num_lines=20
		)
		self.add(ax, graph, slopes)

```

  

同上面 get_area 用法类似，不赘述。

  

### CoordinateSystem.get_x_axis_label()

  

通过 axes 得到x 轴标签，`CoordinateSystem.get_y_axis_label()`用法类似，不过多赘述。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2016.07.18.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		ax = Axes(y_range=[-1, 7])
		graph = ax.plot(lambda x: 1 / 4 * x ** 2, color=BLUE)
		x_label = ax.get_x_axis_label(Tex("$x$-values"))
		self.add(ax, graph, x_label)

```

  

可选参数：

- **label** – 标签。默认为[`MathTex`](https://docs.manim.community/en/stable/reference/manim.mobject.text.tex_mobject.MathTex.html#manim.mobject.text.tex_mobject.MathTex "manim.mobject.text.tex_mobject.MathTex")for`str`和`float`input。

- **edge** – 默认情况下，将添加标签的 y 轴边缘`UR`。

- **direction** – 默认情况下，允许从边缘进一步定位标签`UR`

- **buff** – 标签与线的距离。

  

### CoordinateSystem.input_to_graph_point()

  

返回对应函数上某个点的坐标。

  

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2016.11.59.png)

  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		ax = Axes(y_range=[-1, 7])
		graph = ax.plot(lambda x: 1 / 4 * x ** 2, color=BLUE)
		pos = ax.input_to_graph_point(x=PI, graph=graph)
		square = Square(side_length=1).move_to(pos)
		
		self.add(ax, graph, square)

```

  

### CoordinateSystem.plot()

  

返回一个曲线，并不会直接绘制需自行赋值并添加。

  

参数：

- **function** – 用于构造的函数[`ParametricFunction`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.functions.ParametricFunction.html#manim.mobject.graphing.functions.ParametricFunction "manim.mobject.graphing.functions.ParametricFunction")。

- **x_range**  – 曲线沿轴的范围。`x_range = [x_min, x_max, x_step]`

- **use_vectorized**  – 是否将生成的 t 值数组传递给函数。仅当你的函数支持时才使用此选项。输出应为形状为`[y_0, y_1, ...]`

- **colorscale**  – 函数的颜色。此参数为可选参数，用于根据值对函数着色。传递颜色列表和 colorscale_axis 将根据 y 值对函数着色。传递表单中的元组列表， 允许用户定义颜色过渡的枢轴。`(color, pivot)`

- **colorscale_axis**  – 定义应用颜色比例的轴（0 = x，1 = y），默认为 y 轴（1）。

- **kwargs** – 要传递给的附加参数[`ParametricFunction`](https://docs.manim.community/en/stable/reference/manim.mobject.graphing.functions.ParametricFunction.html#manim.mobject.graphing.functions.ParametricFunction "manim.mobject.graphing.functions.ParametricFunction")。

  

来简单绘制一条 log 曲线，如果定义域内的值难以取到，效果可能不尽人意，这时可以适当调整 use_smoothing 参数来达到更好的效果。

  

![这是一张图片](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2016.24.42.png)

  
  
  

```python

from manim import *

class SingleScene(Scene):
	def construct(self):
		ax = Axes(
			x_range=[0.001, 6],
			y_range=[-8, 2],
			x_length=10,
			y_length=5,
		)
		
		graph = ax.plot(lambda x: np.log(x), color=BLUE, use_smoothing=False)
		pos = ax.input_to_graph_point(x=PI, graph=graph)
		dot = Dot(pos, color=RED)
		square = Square(side_length=1).move_to(pos)
		self.play(Create(ax), Write(graph), Write(square), Write(dot))

```

## ValueTracker 绘制

ValueTracker 解决了什么问题：在绘制动画时，我们往往使用 `self.play()` 方法一步一步的绘制。如果一个数值在每一帧都需要修改其动画怎么办？这就需要用到 valueTracker 来自动更新动画了。

在没有 tracker 时，我们操作一个矩形移动需要直接操作矩形本身。有了 tracker 之后我们就能像开发游戏一样，直接操作一个变量，系统自动根据变量更改矩形的动画。简而言之，大大方便了动画的制作。

**valueTracker** 仅仅是一个用于存储值的简单对象，将其理解为一个变量就行。

我们可以为一个 Mobjects 添加一个更新函数，只要函数中的 tracker 的值发生了改变，manim 会自动为我们执行函数中的逻辑。例如：

```python
dot = Dot().add_updater(
	lambda x: x.move_to(tracker.points)
)
```

这里为 dot 设置了一个更新函数，每当函数中的 tracker 发生改变时，自动执行函数中的逻辑 move_to。

> 为了简单起见，这里不介绍`complexValueTracker`，实际上原理相似，自行查看官方文档即可。

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-11%2016.57.48.png)

```python
from manim import *

class SingleScene(Scene):
    def construct(self):
        number_line = NumberLine()

        tracker = ValueTracker(0)

        arrow = Vector(DOWN)
        arrow.add_updater(
            lambda m: m.next_to(
                number_line.n2p(tracker.get_value()),
                direction=UP
            )
        )
        label = MathTex('x').add_updater(
            lambda l: l.next_to(arrow, UP)
        )

        self.add(number_line, arrow, label)
        self.play(tracker.animate.set_value(2))
        self.play(tracker.animate.set_value(-2))
```


可以看到，每个绑定了`add_updater`的对象都会在 tracker 更新的时候执行 lambda 中的工作。在 self.play 中，我们的 tracker 修改数值是一个插值动画，这意味着 tracker 的值不是一瞬间就修改完成的，而是有过程的。相应的就可以调整 tracker 改变的速度和时间来满足更好的需要。


