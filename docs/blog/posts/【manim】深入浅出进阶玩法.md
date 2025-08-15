---
title: 【manim】Animations 详解
author: Sy
index_img: https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-10%2016.58.38.png
cover: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
thumbnail: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
sticky: 0
expires: ''
excerpt: manim 从入门到精通
tags:
  - ' manim'
categories:
  - manim
date: 2025-07-10 21:59:00
---

## Animations

`Animations`很重要，用于操作`Manim`中的动画行为。比如下面的`self.play()`方法中，`Create()`就是`Animtions`类下的一个方法，下面来详细说说有哪些。

```python
self.play(Create(circle))
```
<!-- more -->

### animation

#### Add

定义：`manim.animation.animation.Add`

该方法会立即在场景中添加`Mobject`。

> 完整的例子参考官方文档，这里只写简要的使用介绍。下文如此。

示例：
```python
c = Circle()
self.play(Succession(Add(c)),Create(c))
```

该方法区别于`self.add()`，特点是放在`Succession()` 中使用。

### composition

一次显示多个动画的工具，部分方法我感觉没什么用本文就不记录了，如有需要自行查阅官方文档。

> 其实也看不太懂，以后研究明白再记录吧。

| 方法名                                                                                                                                                                                                          | 功能                                                                                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`AnimationGroup`](https://docs.manim.community/en/stable/reference/manim.animation.composition.AnimationGroup.html#manim.animation.composition.AnimationGroup "manim.animation.composition.AnimationGroup") | Plays a group or series of [`Animation`](https://docs.manim.community/en/stable/reference/manim.animation.animation.Animation.html#manim.animation.animation.Animation "manim.animation.animation.Animation").                                 |
| [`LaggedStart`](https://docs.manim.community/en/stable/reference/manim.animation.composition.LaggedStart.html#manim.animation.composition.LaggedStart "manim.animation.composition.LaggedStart")             | Adjusts the timing of a series of [`Animation`](https://docs.manim.community/en/stable/reference/manim.animation.animation.Animation.html#manim.animation.animation.Animation "manim.animation.animation.Animation") according to `lag_ratio`. |
| [`LaggedStartMap`](https://docs.manim.community/en/stable/reference/manim.animation.composition.LaggedStartMap.html#manim.animation.composition.LaggedStartMap "manim.animation.composition.LaggedStartMap") | Plays a series of [`Animation`](https://docs.manim.community/en/stable/reference/manim.animation.animation.Animation.html#manim.animation.animation.Animation "manim.animation.animation.Animation") while mapping a function to submobjects.  |
| [`Succession`](https://docs.manim.community/en/stable/reference/manim.animation.composition.Succession.html#manim.animation.composition.Succession "manim.animation.composition.Succession")                 | Plays a series of animations in succession.                                                                                                                                                                                                    |

#### LaggedStartMap

`LaggedStart(xxx, xxx, lage_ratio=0.25, run_time=4)`，`lage_ratio`为延迟因子，`run_time`为动画时间。

让动画延迟执行，需要注意 `LaggedStart()`中不能直接放入`Circle`这类`Mobject`，报错遇到了别慌。

```python
from manim import *

class SingleScene(Scene):
   def construct(self):
      dot1 = Dot(point=LEFT * 2 + UP, radius=0.16)
      dot2 = Dot(point=LEFT * 2, radius=0.16)
      dot3 = Dot(point=LEFT * 2 + DOWN, radius=0.16)
      self.play(
         LaggedStart(
            dot1.animate.shift(RIGHT * 4),
            dot2.animate.shift(RIGHT * 4),
            dot3.animate.shift(RIGHT * 4),
            lag_ratio=.25,
            run_time=4
         )
      )
```



#### Succession

正常的`self.play()`中写入多个动画会同时执行而不分先后。为了克服这一点，使用`Succession()`可以将多个动画串在一起为一个组，每个`Succession`组内的动画串行执行。

```python
class SuccessionExample(Scene):
    def construct(self):
        dot1 = Dot(point=LEFT * 2 + UP * 2, radius=0.16, color=BLUE)
        dot2 = Dot(point=LEFT * 2 + DOWN * 2, radius=0.16, color=MAROON)
        dot3 = Dot(point=RIGHT * 2 + DOWN * 2, radius=0.16, color=GREEN)
        dot4 = Dot(point=RIGHT * 2 + UP * 2, radius=0.16, color=YELLOW)
        self.add(dot1, dot2, dot3, dot4)

        self.play(Succession(
            dot1.animate.move_to(dot2),
            dot2.animate.move_to(dot3),
            dot3.animate.move_to(dot4),
            dot4.animate.move_to(dot1)
        ))
```


### creation

`creation`下的方法用于显示或移除动画，极其实用。


官网文档地址对照表：

| 方法名                                                                                                                                                                                                                                         | 功能                                                                                                                                                                                                                                                                                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`AddTextLetterByLetter`](https://docs.manim.community/en/stable/reference/manim.animation.creation.AddTextLetterByLetter.html#manim.animation.creation.AddTextLetterByLetter "manim.animation.creation.AddTextLetterByLetter")             | [`Text`](https://docs.manim.community/en/stable/reference/manim.mobject.text.text_mobject.Text.html#manim.mobject.text.text_mobject.Text "manim.mobject.text.text_mobject.Text")现场逐字逐句展示。                                                                                                                                                                                                                          |
| [`AddTextWordByWord`](https://docs.manim.community/en/stable/reference/manim.animation.creation.AddTextWordByWord.html#manim.animation.creation.AddTextWordByWord "manim.animation.creation.AddTextWordByWord")                             | [`Text`](https://docs.manim.community/en/stable/reference/manim.mobject.text.text_mobject.Text.html#manim.mobject.text.text_mobject.Text "manim.mobject.text.text_mobject.Text")现场逐字逐句展示。（已废弃）                                                                                                                                                                                                                     |
| [`Create`](https://docs.manim.community/en/stable/reference/manim.animation.creation.Create.html#manim.animation.creation.Create "manim.animation.creation.创建")                                                                             | 逐步显示 VMobject。                                                                                                                                                                                                                                                                                                                                                                                                     |
| [`DrawBorderThenFill`](https://docs.manim.community/en/stable/reference/manim.animation.creation.DrawBorderThenFill.html#manim.animation.creation.DrawBorderThenFill "manim.动画.创建.DrawBorderThenFill")                                      | 先绘制边框，然后显示填充。                                                                                                                                                                                                                                                                                                                                                                                                      |
| [`RemoveTextLetterByLetter`](https://docs.manim.community/en/stable/reference/manim.animation.creation.RemoveTextLetterByLetter.html#manim.animation.creation.RemoveTextLetterByLetter "manim.animation.creation.RemoveTextLetterByLetter") | [`Text`](https://docs.manim.community/en/stable/reference/manim.mobject.text.text_mobject.Text.html#manim.mobject.text.text_mobject.Text "manim.mobject.text.text_mobject.Text")从场景中逐个字母地删除。                                                                                                                                                                                                                       |
| [`ShowIncreasingSubsets`](https://docs.manim.community/en/stable/reference/manim.animation.creation.ShowIncreasingSubsets.html#manim.animation.creation.ShowIncreasingSubsets "manim.animation.creation.ShowIncreasingSubsets")             | 一次显示一个子对象，所有先前的子对象仍显示在屏幕上。                                                                                                                                                                                                                                                                                                                                                                                         |
| [`ShowPartial`](https://docs.manim.community/en/stable/reference/manim.animation.creation.ShowPartial.html#manim.animation.creation.ShowPartial "manim.animation.creation.ShowPartial")                                                     | 部分显示 VMobject 的动画的抽象类。                                                                                                                                                                                                                                                                                                                                                                                             |
| [`ShowSubmobjectsOneByOne`](https://docs.manim.community/en/stable/reference/manim.animation.creation.ShowSubmobjectsOneByOne.html#manim.animation.creation.ShowSubmobjectsOneByOne "manim.animation.creation.ShowSubmobjectsOneByOne")     | 一次显示一个子对象，从屏幕上删除所有先前显示的子对象。                                                                                                                                                                                                                                                                                                                                                                                        |
| [`SpiralIn`](https://docs.manim.community/en/stable/reference/manim.animation.creation.SpiralIn.html#manim.animation.creation.SpiralIn "manim.animation.creation.SpiralIn")                                                                 | 创建 Mobject，其中子 Mobject 以螺旋轨迹飞入。                                                                                                                                                                                                                                                                                                                                                                                    |
| [`TypeWithCursor`](https://docs.manim.community/en/stable/reference/manim.animation.creation.TypeWithCursor.html#manim.animation.creation.TypeWithCursor "manim.animation.creation.TypeWithCursor")                                         | 类似于[`AddTextLetterByLetter`](https://docs.manim.community/en/stable/reference/manim.animation.creation.AddTextLetterByLetter.html#manim.animation.creation.AddTextLetterByLetter "manim.animation.creation.AddTextLetterByLetter")，但在末尾有一个额外的光标 mobject。                                                                                                                                                           |
| [`Uncreate`](https://docs.manim.community/en/stable/reference/manim.animation.creation.Uncreate.html#manim.animation.creation.Uncreate "manim.animation.creation.取消创建")                                                                     | 类似[`Create`](https://docs.manim.community/en/stable/reference/manim.animation.creation.Create.html#manim.animation.creation.Create "manim.animation.creation.创建")，但反过来。                                                                                                                                                                                                                                            |
| [`UntypeWithCursor`](https://docs.manim.community/en/stable/reference/manim.animation.creation.UntypeWithCursor.html#manim.animation.creation.UntypeWithCursor "manim.animation.creation.UntypeWithCursor")                                 | 类似于[`RemoveTextLetterByLetter`](https://docs.manim.community/en/stable/reference/manim.animation.creation.RemoveTextLetterByLetter.html#manim.animation.creation.RemoveTextLetterByLetter "manim.animation.creation.RemoveTextLetterByLetter")，但在末尾有一个额外的光标 mobject。                                                                                                                                               |
| [`Unwrite`](https://docs.manim.community/en/stable/reference/manim.animation.creation.Unwrite.html#manim.animation.creation.Unwrite "manim.animation.creation.Unwrite")                                                                     | 模拟用手擦除 a[`Text`](https://docs.manim.community/en/stable/reference/manim.mobject.text.text_mobject.Text.html#manim.mobject.text.text_mobject.Text "manim.mobject.text.text_mobject.Text")或 a [`VMobject`](https://docs.manim.community/en/stable/reference/manim.mobject.types.vectorized_mobject.VMobject.html#manim.mobject.types.vectorized_mobject.VMobject "manim.mobject.types.vectorized_mobject.VMobject")。 |
| [`Write`](https://docs.manim.community/en/stable/reference/manim.animation.creation.Write.html#manim.animation.creation.Write "manim.animation.creation.Write")                                                                             | 模拟手写[`Text`](https://docs.manim.community/en/stable/reference/manim.mobject.text.text_mobject.Text.html#manim.mobject.text.text_mobject.Text "manim.mobject.text.text_mobject.Text")或手画[`VMobject`](https://docs.manim.community/en/stable/reference/manim.mobject.types.vectorized_mobject.VMobject.html#manim.mobject.types.vectorized_mobject.VMobject "manim.mobject.types.vectorized_mobject.VMobject")。      |

#### AddTextLetterByLetter

方法名：`manim.animation.creation.AddTextLetterByLetter`

作用：逐字显示文本。

```python
t = Text("你好，世界")
self.play(AddTextLetterByLetter(t))
```


#### Create

逐步显示一个`VMobject`。

```python
from manim import *

class CreateScene(Scene):
    def construct(self):
        self.play(Create(Square()))
```


#### DrawBorderThenFill

> 下文省略方法名，自己去官网看吧，咱也不记这个。

先画框架再填充图形。




```python
from manim import *

class ShowDrawBorderThenFill(Scene):
    def construct(self):
        self.play(DrawBorderThenFill(Square(fill_opacity=1, fill_color=ORANGE)))
```

#### RemoveTextLetterByLetter

逐字移除字符。

```python
from manim import *

class SingleScene(Scene):
   def construct(self):
      t = Text("你好，世界")
      self.play(RemoveTextLetterByLetter(t))
```

#### SpiralIn

螺旋显示对象。


```python
from manim import *

class SpiralInExample(Scene):
    def construct(self):
        pi = MathTex(r"\pi").scale(7)
        pi.shift(2.25 * LEFT + 1.5 * UP)
        circle = Circle(color=GREEN_C, fill_opacity=1).shift(LEFT)
        square = Square(color=BLUE_D, fill_opacity=1).shift(UP)
        shapes = VGroup(pi, circle, square)
        self.play(SpiralIn(shapes))
```

#### TypeWithCursor

带有光标的输入。


```python
from manim import *

class InsertingTextExample(Scene):
    def construct(self):
        text = Text("Inserting", color=PURPLE).scale(1.5).to_edge(LEFT)
        cursor = Rectangle(
            color = GREY_A,
            fill_color = GREY_A,
            fill_opacity = 1.0,
            height = 1.1,
            width = 0.5,
        ).move_to(text[0]) # Position the cursor

        self.play(TypeWithCursor(text, cursor))
        self.play(Blink(cursor, blinks=2))
```

#### Uncreate

与`Create`相反。


```python
from manim import *

class ShowUncreate(Scene):
    def construct(self):
        self.play(Uncreate(Square()))
```

#### UntypeWithCursor

和`TypeWithCursor`相反。


```python
from manim import *

class DeletingTextExample(Scene):
    def construct(self):
        text = Text("Deleting", color=PURPLE).scale(1.5).to_edge(LEFT)
        cursor = Rectangle(
            color = GREY_A,
            fill_color = GREY_A,
            fill_opacity = 1.0,
            height = 1.1,
            width = 0.5,
        ).move_to(text[0]) # Position the cursor

        self.play(UntypeWithCursor(text, cursor))
        self.play(Blink(cursor, blinks=2))
```

#### Unwrite

与`Write`相反。


```python
from manim import *

class UnwriteReverseTrue(Scene):
    def construct(self):
        text = Tex("Alice and Bob").scale(3)
        self.add(text)
        self.play(Unwrite(text))
```

也可以另方向相反，只需要加上参数，`reverse=False`。

#### Write

网上很常见的方法，很多创作者都在用该方法显示文字。


```python
from manim import *

class ShowWrite(Scene):
    def construct(self):
        self.play(Write(Text("Hello", font_size=144)))
```

可选参数：
- `reverse=True`
- `remover=False`

### fading

淡入和淡出，比较常用在过渡动画。


```python
from manim import *

class Fading(Scene):
    def construct(self):
        tex_in = Tex("Fade", "In").scale(3)
        tex_out = Tex("Fade", "Out").scale(3)
        self.play(FadeIn(tex_in, shift=DOWN, scale=0.66))
        self.play(ReplacementTransform(tex_in, tex_out))
        self.play(FadeOut(tex_out, shift=DOWN * 2, scale=1.5))
```

#### FadeIn

淡入，可选参数：
- **mobjects** ([_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")) 传入被作用动画的Mobject 对象
- **shift** – 淡入时的偏移
- **target_position** – 淡入开始时的位置，回逐渐过渡到正常位置
- **scale** – 淡入开始时的大小，会逐渐过渡到正常大小

```python
from manim import *

class FadeInExample(Scene):
    def construct(self):
        dot = Dot(UP * 2 + LEFT)
        self.add(dot)
        tex = Tex(
            "FadeIn with ", "shift ", r" or target\_position", " and scale"
        ).scale(1)
        animations = [
            FadeIn(tex[0]),
            FadeIn(tex[1], shift=DOWN),
            FadeIn(tex[2], target_position=dot),
            FadeIn(tex[3], scale=1.5),
        ]
        self.play(AnimationGroup(*animations, lag_ratio=0.5))
```


#### FadeOut

淡出，参数同上，这里直接贴英文版，更详尽一些。
- **mobjects** ([_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")) – The mobjects to be faded out.
- **shift** – The vector by which the mobject shifts while being faded out.
- **target_position** – The position to which the mobject moves while being faded out. In case another mobject is given as target position, its center is used.
- **scale** – The factor by which the mobject is scaled while being faded out.


演示代码：
```python
from manim import *

class FadeInExample(Scene):
    def construct(self):
        dot = Dot(UP * 2 + LEFT)
        self.add(dot)
        tex = Tex(
            "FadeOut with ", "shift ", r" or target\_position", " and scale"
        ).scale(1)
        animations = [
            FadeOut(tex[0]),
            FadeOut(tex[1], shift=DOWN),
            FadeOut(tex[2], target_position=dot),
            FadeOut(tex[3], scale=0.5),
        ]
        self.play(AnimationGroup(*animations, lag_ratio=0.5))
```


### growing

对象从点开始生长来引入场景。


```python
from manim import *

class Growing(Scene):
    def construct(self):
        square = Square()
        circle = Circle()
        triangle = Triangle()
        arrow = Arrow(LEFT, RIGHT)
        star = Star()

        VGroup(square, circle, triangle).set_x(0).arrange(buff=1.5).set_y(2)
        VGroup(arrow, star).move_to(DOWN).set_x(0).arrange(buff=1.5).set_y(-2)

        self.play(GrowFromPoint(square, ORIGIN))
        self.play(GrowFromCenter(circle))
        self.play(GrowFromEdge(triangle, DOWN))
        self.play(GrowArrow(arrow))
        self.play(SpinInFromNothing(star))
```

官方目录：

| 方法名                                                                                                                                                                                                          | 功能                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`GrowArrow`](https://docs.manim.community/en/stable/reference/manim.animation.growing.GrowArrow.html#manim.animation.growing.GrowArrow "manim.animation.growing.GrowArrow")                                 | [`Arrow`](https://docs.manim.community/en/stable/reference/manim.mobject.geometry.line.Arrow.html#manim.mobject.geometry.line.Arrow "manim.mobject.geometry.line.Arrow")通过从其起点向其终点生长来引入。 |
| [`GrowFromCenter`](https://docs.manim.community/en/stable/reference/manim.animation.growing.GrowFromCenter.html#manim.animation.growing.GrowFromCenter "manim.动画.生长.从中心生长")                                  | [`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")通过从中心生长来引入。                |
| [`GrowFromEdge`](https://docs.manim.community/en/stable/reference/manim.animation.growing.GrowFromEdge.html#manim.animation.growing.GrowFromEdge "manim.animation.growing.GrowFromEdge")                     | [`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")通过从其边界框边缘之一增长来引入。          |
| [`GrowFromPoint`](https://docs.manim.community/en/stable/reference/manim.animation.growing.GrowFromPoint.html#manim.animation.growing.GrowFromPoint "manim.animation.growing.GrowFromPoint")                 | [`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")通过从一个点扩展它来引入一个。            |
| [`SpinInFromNothing`](https://docs.manim.community/en/stable/reference/manim.animation.growing.SpinInFromNothing.html#manim.animation.growing.SpinInFromNothing "manim.animation.growing.SpinInFromNothing") | 引入[`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")旋转并从中心开始生长。              |
#### GrowArrow

`Arrow`通过其起点和终点来引入。

参数：
- **arrow** ([_Arrow_](https://docs.manim.community/en/stable/reference/manim.mobject.geometry.line.Arrow.html#manim.mobject.geometry.line.Arrow "manim.mobject.geometry.line.Arrow")) – 要引入的箭头。
- **point_color** (_str_) – 完全填充前的初始颜色，留空将自动匹配箭头颜色。


```python
from manim import *

class GrowArrowExample(Scene):
    def construct(self):
        arrows = [Arrow(2 * LEFT, 2 * RIGHT), Arrow(2 * DR, 2 * UL)]
        VGroup(*arrows).set_x(0).arrange(buff=2)
        self.play(GrowArrow(arrows[0]))
        self.play(GrowArrow(arrows[1], point_color=RED))
```


#### GrowFromCenter

让`Mobject`从中心开始生长。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要引入的 mobjects。
- **point_color** ( _str_ ) – 对象在增长到其实际大小之前的初始颜色。留空可匹配对象的颜色。


```python
from manim import *

class GrowFromCenterExample(Scene):
    def construct(self):
        squares = [Square() for _ in range(2)]
        VGroup(*squares).set_x(0).arrange(buff=2)
        self.play(GrowFromCenter(squares[0]))
        self.play(GrowFromCenter(squares[1], point_color=RED))
```

#### GrowFromEdge

从边界框边缘之一开始增长引入。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要引入的 `mobjects`。
- **edge** ( _np.ndarray_ ) – 寻找 `mobject` 边界框边缘的方向。
- **point_color** ( _str_ ) – 对象在增长到其实际大小之前的初始颜色。留空可匹配对象的颜色。



```python
from manim import *

class GrowFromEdgeExample(Scene):
    def construct(self):
        squares = [Square() for _ in range(4)]
        VGroup(*squares).set_x(0).arrange(buff=1)
        self.play(GrowFromEdge(squares[0], DOWN))
        self.play(GrowFromEdge(squares[1], RIGHT))
        self.play(GrowFromEdge(squares[2], UR))
        self.play(GrowFromEdge(squares[3], UP, point_color=RED))
```

| 其余可选属性      |
| ----------- |
| `path_arc`  |
| `path_func` |
| `run_time`  |

#### GrowFromPoint

从点开始生长来引入。可以是外部的点。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要引入的 `mobjects`。
- **point**（_np.ndarray_） – `mobject` 生长的点。
- **point_color** ( _str_ ) – 对象在增长到其实际大小之前的初始颜色。留空可匹配对象的颜色。


```python
from manim import *

class GrowFromPointExample(Scene):
    def construct(self):
        dot = Dot(3 * UR, color=GREEN)
        squares = [Square() for _ in range(4)]
        VGroup(*squares).set_x(0).arrange(buff=1)
        self.add(dot)
        self.play(GrowFromPoint(squares[0], ORIGIN))
        self.play(GrowFromPoint(squares[1], [-2, 2, 0]))
        self.play(GrowFromPoint(squares[2], [3, -2, 0], RED))
        self.play(GrowFromPoint(squares[3], dot, dot.get_color()))
```

说明：`ORIGIN`是屏幕中心点，是`manim`中的原点。


#### SpinInFromNothing

从中心开始旋转生长。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要引入的 mobjects。
- **angle**（_浮点数_） – 物体达到其最大尺寸前的旋转角度。例如，$2*PI$ 表示物体在完全进入物体之前会旋转一圈。
- **point_color** ( _str_ ) – 对象在增长到其实际大小之前的初始颜色。留空可匹配对象的颜色。


```python
from manim import *

class SpinInFromNothingExample(Scene):
    def construct(self):
        squares = [Square() for _ in range(3)]
        VGroup(*squares).set_x(0).arrange(buff=2)
        self.play(SpinInFromNothing(squares[0]))
        self.play(SpinInFromNothing(squares[1], angle=2 * PI))
        self.play(SpinInFromNothing(squares[2], point_color=RED))
```

### indication

吸引人注意的动画。


```python
from manim import *

class Indications(Scene):
    def construct(self):
        indications = [ApplyWave,Circumscribe,Flash,FocusOn,Indicate,ShowPassingFlash,Wiggle]
        names = [Tex(i.__name__).scale(3) for i in indications]

        self.add(names[0])
        for i in range(len(names)):
            if indications[i] is Flash:
                self.play(Flash(UP))
            elif indications[i] is ShowPassingFlash:
                self.play(ShowPassingFlash(Underline(names[i])))
            else:
                self.play(indications[i](names[i]))
            self.play(AnimationGroup(
                FadeOut(names[i], shift=UP*1.5),
                FadeIn(names[(i+1)%len(names)], shift=UP*1.5),
            ))
```

上述代码遍历所有的`indications`方法，通过`__name__`拿到其命名存入`names`数组。遍历`indications`通过`is`判断其类型播放对应动画。

 官方目录：

| 方法名                                                                                                                                                                                                                                                                                                           | 功能                        |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- |
| [`ApplyWave`](https://docs.manim.community/en/stable/reference/manim.animation.indication.ApplyWave.html#manim.animation.indication.ApplyWave "manim.动画.指示.ApplyWave")                                                                                                                                        | 通过 Mobject 发送波并暂时扭曲它。     |
| [`Blink`](https://docs.manim.community/en/stable/reference/manim.animation.indication.Blink.html#manim.animation.indication.Blink "manim.animation.indication.闪烁")                                                                                                                                            | 使物体闪烁。                    |
| [`Circumscribe`](https://docs.manim.community/en/stable/reference/manim.animation.indication.Circumscribe.html#manim.animation.indication.Circumscribe "manim.animation.indication.外切")                                                                                                                       | 在对象周围画一条临时线。              |
| [`Flash`](https://docs.manim.community/en/stable/reference/manim.animation.indication.Flash.html#manim.animation.indication.Flash "manim.动画.指示.Flash")                                                                                                                                                        | 向各个方向发出线路。                |
| [`FocusOn`](https://docs.manim.community/en/stable/reference/manim.animation.indication.FocusOn.html#manim.animation.indication.FocusOn "manim.动画.指示.FocusOn")                                                                                                                                                | 将聚光灯缩小到一个位置。              |
| [`Indicate`](https://docs.manim.community/en/stable/reference/manim.animation.indication.Indicate.html#manim.animation.indication.Indicate "manim.animation.indication.指示")                                                                                                                                   | 通过临时调整大小和重新着色来指示 Mobject。 |
| [`ShowPassingFlash`](https://docs.manim.community/en/stable/reference/manim.animation.indication.ShowPassingFlash.html#manim.animation.indication.ShowPassingFlash "manim.animation.indication.ShowPassingFlash")                                                                                             | 每帧仅显示 VMobject 的一小部分。     |
| [`ShowPassingFlashWithThinningStrokeWidth`](https://docs.manim.community/en/stable/reference/manim.animation.indication.ShowPassingFlashWithThinningStrokeWidth.html#manim.animation.indication.ShowPassingFlashWithThinningStrokeWidth "manim.animation.indication.ShowPassingFlashWithThinningStrokeWidth") |                           |
| [`Wiggle`](https://docs.manim.community/en/stable/reference/manim.animation.indication.Wiggle.html#manim.animation.indication.Wiggle "manim.动画.指示.摆动")                                                                                                                                                        | 摆动 Mobject。               |
#### ApplyWave

波浪一般扭曲一个`Mobject`。

参数:
- **mobject** ([_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")) – 需要被扭曲的`Mobject`对象
- **direction** (_np.ndarray_) – 波浪推动形状点的方向
- **amplitude** (_float_) – 形状的距离点发生偏移
- **wave_func** (Callable[[float], float]) – 定义一个波侧形状的函数
- **time_width** (_float_) – 相对于 mobject 宽度的波的长度
- **ripples** (_int_) – 波浪的涟漪数量
- **run_time** (_float_) – 动画的持续时间


```python
from manim import *

class ApplyingWaves(Scene):
    def construct(self):
        tex = Tex("WaveWaveWaveWaveWave").scale(2)
        self.play(ApplyWave(tex))
        self.play(ApplyWave(
            tex,
            direction=RIGHT,
            time_width=0.5,
            amplitude=0.3
        ))
        self.play(ApplyWave(
            tex,
            rate_func=linear,
            ripples=4
        ))
```

#### Blink

闪烁，一闪一闪什么来着...

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要闪烁的 `mobject`。
- **time_on** ( _float_ ) – mobject 闪烁一次的持续时间。
- **time_off** ( _float_ ) – mobject 隐藏一次闪烁的持续时间。
- **闪烁**（_int_）- 闪烁次数
- **hide_at_end** ( _bool_ ) – 是否在动画结束时隐藏 mobject。
- **kwargs** – 要传递给[`Succession`](https://docs.manim.community/en/stable/reference/manim.animation.composition.Succession.html#manim.animation.composition.Succession "manim.动画.组合.继承")构造函数的附加参数。


```python
from manim import *

class BlinkingExample(Scene):
    def construct(self):
        text = Text("Blinking").scale(1.5)
        self.add(text)
        self.play(Blink(text, blinks=3))
```

#### Circumscribe

在对象周围画一条临时的线。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要外接的 `mobject`。
- **shape**（_类型_） – 包围给定对象的形状。可以 [`Rectangle`](https://docs.manim.community/en/stable/reference/manim.mobject.geometry.polygram.Rectangle.html#manim.mobject.geometry.polygram.Rectangle "manim.mobject.geometry.polygram.Rectangle")或[`Circle`](https://docs.manim.community/en/stable/reference/manim.mobject.geometry.arc.Circle.html#manim.mobject.geometry.arc.Circle "manim.mobject.geometry.arc.Circle")
- **fade_in** – 是否使周围的形状淡入。否则将被绘制。
- **fade_out** – 是否使周围的形状淡出。否则将不绘制。
- **time_width** – 绘制和取消绘制的 `time_width`。如果 `fade_in` 或 `fade_out` 为 `True`  ，则忽略该值。
- **buff**（_float_） – 周围形状和给定​​ `mobject` 之间的距离。
- **color**（[_ParsableManimColor_](https://docs.manim.community/en/stable/reference/manim.utils.color.core.html#manim.utils.color.core.ParsableManimColor "manim.utils.color.core.ParsableManimColor")） – 周围形状的颜色。
- **run_time** – 整个动画的持续时间。
- **kwargs** – 传递给[`Succession`](https://docs.manim.community/en/stable/reference/manim.animation.composition.Succession.html#manim.animation.composition.Succession "manim.动画.组合.继承")构造函数的附加参数。


```python
from manim import *

class UsingCircumscribe(Scene):
    def construct(self):
        lbl = Tex(r"Circum-\\scribe").scale(2)
        self.add(lbl)
        self.play(Circumscribe(lbl))
        self.play(Circumscribe(lbl, Circle))
        self.play(Circumscribe(lbl, fade_out=True))
        self.play(Circumscribe(lbl, time_width=2))
        self.play(Circumscribe(lbl, Circle, True))
```

一个更加简单的例子：

```python
from manim import *

class SingleScene(Scene):
   def construct(self):
      t = Text("Hello")
      self.add(t)
      self.play(Circumscribe(t, Circle))
```


#### Flash

向各个方向发出线路。

参数：
- **point** ( _np.ndarray_ _|_ [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 闪光线的中心。如果是，`Mobject`则使用其中心。
- **line_length** ( _float_ ) – 闪光线的长度。
- **num_lines** ( _int_ ) – 闪光线的数量。
- **flash_radius** ( _float_ ) –闪光线起始点的距离。
- **line_stroke_width** ( _int_ ) - 闪光线的笔触宽度。
- **color**（_str_） – 闪光线的颜色。
- **time_width** ( _float_ ) – 闪光线的时间宽度。`ShowPassingFlash`更多详情，请参阅。
- **run_time**（_float_） – 动画的持续时间。
- **kwargs** – 传递给[`Succession`](https://docs.manim.community/en/stable/reference/manim.animation.composition.Succession.html#manim.animation.composition.Succession "manim.动画.组合.继承")构造函数的附加参数。


```python
from manim import *

class UsingFlash(Scene):
    def construct(self):
        dot = Dot(color=YELLOW).shift(DOWN)
        self.add(Tex("Flash the dot below:"), dot)
        self.play(Flash(dot))
        self.wait()
```

高级例子：


```python
from manim import *

class FlashOnCircle(Scene):
    def construct(self):
        radius = 2
        circle = Circle(radius)
        self.add(circle)
        self.play(Flash(
            circle, line_length=1,
            num_lines=30, color=RED,
            flash_radius=radius+SMALL_BUFF,
            time_width=0.3, run_time=2,
            rate_func = rush_from
        ))
```


#### FocusOn

将灯光缩小到一个位置，类似于聚光灯。

参数：

- **focus_point** ( _np.ndarray_ _|_ [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 聚光灯收缩的点。如果是`Mobject`则使用其中心。
- **opacity**（_浮点数_） – 聚光灯的不透明度。
- **color**（_str_） – 聚光灯的颜色。
- **run_time**（_float_） – 动画的持续时间。


```python
from manim import *

class UsingFocusOn(Scene):
    def construct(self):
        dot = Dot(color=YELLOW).shift(DOWN)
        self.add(Tex("Focusing on the dot below:"), dot)
        self.play(FocusOn(dot))
        self.wait()
```

#### Indicate

临时调整大小和重新着色来强调 `Mobject`。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要指示的 mobject。
- **scale_factor** ( _float_ ) – 对象时间缩放的因子
- **color**（_str_） – mobject 暂时呈现的颜色。
- **rate_func** ( _Callable_ [/[float,float|None],np.ndarray]) – 定义每个时间点的动画进度的函数。
- **kwargs** – 传递给[`Succession`](https://docs.manim.community/en/stable/reference/manim.animation.composition.Succession.html#manim.animation.composition.Succession "manim.动画.组合.继承")构造函数的附加参数。


```python
from manim import *

class UsingIndicate(Scene):
    def construct(self):
        tex = Tex("Indicate").scale(3)
        self.play(Indicate(tex))
        self.wait()
```

#### ShowPassingFlash

类似于笔触动画，每一帧只显示`Mobj` 第一部分。

[^Mobj]: 这里把`Mobject`简写为`Mobj`，下文如此。

参数：
- **mobject** ( [_VMobject_](https://docs.manim.community/en/stable/reference/manim.mobject.types.vectorized_mobject.VMobject.html#manim.mobject.types.vectorized_mobject.VMobject "manim.mobject.types.vectorized_mobject.VMobject") ) – 笔触具有动画效果的 `mobject`。
- **time_width** ( _float_ ) – 相对于笔划长度的碎片长度。


```python
from manim import *

class TimeWidthValues(Scene):
    def construct(self):
        p = RegularPolygon(5, color=DARK_GRAY, stroke_width=6).scale(3)
        lbl = VMobject()
        self.add(p, lbl)
        p = p.copy().set_color(BLUE)
        for time_width in [0.2, 0.5, 1, 2]:
            lbl.become(Tex(r"\texttt{time\_width={{%.1f}}}"%time_width))
            self.play(ShowPassingFlash(
                p.copy().set_color(BLUE),
                run_time=2,
                time_width=time_width
            ))
```

#### Wiggle

摆动`Mobj`。

参数：
- **mobject**（[_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")） – 要摆动的 `mobject`。
- **scale_value** ( _float_ ) – `mobject` 临时缩放的因子。
- **rotation_angle** ( _float_ ) – 摆动角度。
- **n_wiggles** ( _int_ ) – 摆动的次数。
- **scale_about_point** ( _np.ndarray_ _|_ _None_ ) – `mobject` 缩放的点。
- **rotate_about_point** ( _np.ndarray_ _|_ _None_ ) – `mobject` 围绕其旋转的点。
- **run_time** ( _float_ ) – 动画的持续时间。


```python
from manim import *

class ApplyingWaves(Scene):
    def construct(self):
        tex = Tex("Wiggle").scale(3)
        self.play(Wiggle(tex))
        self.wait()
```


### movement

与运动相关的动画。

目录：

> 为避免歧义这里不翻译。

| 方法                                                                                                                                                                                                                                                  | 功能                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| [`ComplexHomotopy`](https://docs.manim.community/en/stable/reference/manim.animation.movement.ComplexHomotopy.html#manim.animation.movement.ComplexHomotopy "manim.animation.movement.ComplexHomotopy")                                             | Complex Homotopy a function Cx[0, 1] to C                |
| [`Homotopy`](https://docs.manim.community/en/stable/reference/manim.animation.movement.Homotopy.html#manim.animation.movement.Homotopy "manim.animation.movement.Homotopy")                                                                         | A Homotopy.                                              |
| [`MoveAlongPath`](https://docs.manim.community/en/stable/reference/manim.animation.movement.MoveAlongPath.html#manim.animation.movement.MoveAlongPath "manim.animation.movement.MoveAlongPath")                                                     | Make one mobject move along the path of another mobject. |
| [`PhaseFlow`](https://docs.manim.community/en/stable/reference/manim.animation.movement.PhaseFlow.html#manim.animation.movement.PhaseFlow "manim.animation.movement.PhaseFlow")                                                                     |                                                          |
| [`SmoothedVectorizedHomotopy`](https://docs.manim.community/en/stable/reference/manim.animation.movement.SmoothedVectorizedHomotopy.html#manim.animation.movement.SmoothedVectorizedHomotopy "manim.animation.movement.SmoothedVectorizedHomotopy") |                                                          |

#### Homotopy

`Latex`不便于粘贴，只好用`Google`翻译截图之：

![官方描述](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-10%2021.17.14.png)


```python
from manim import *

class HomotopyExample(Scene):
    def construct(self):
        square = Square()

        def homotopy(x, y, z, t):
            if t <= 0.25:
                progress = t / 0.25
                return (x, y + progress * 0.2 * np.sin(x), z)
            else:
                wave_progress = (t - 0.25) / 0.75
                return (x, y + 0.2 * np.sin(x + 10 * wave_progress), z)

        self.play(Homotopy(homotopy, square, rate_func= linear, run_time=2))
```

#### MoveAlongPath

使一个 `mobject` 沿着另一个 `mobject` 的路径移动。


```python
from manim import *

class MoveAlongPathExample(Scene):
    def construct(self):
        d1 = Dot().set_color(ORANGE)
        l1 = Line(LEFT, RIGHT)
        l2 = VMobject()
        self.add(d1, l1, l2)
        l2.add_updater(lambda x: x.become(Line(LEFT, d1.get_center()).set_color(ORANGE)))
        self.play(MoveAlongPath(d1, l1), rate_func=linear)
```


### numbers

改变数字的动画。

> 官方给出的说明较为简略，以文档为准。

 目录：

| 方法名                                                                                                                                                                                                                      |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`ChangeDecimalToValue`](https://docs.manim.community/en/stable/reference/manim.animation.numbers.ChangeDecimalToValue.html#manim.animation.numbers.ChangeDecimalToValue "manim.animation.numbers.ChangeDecimalToValue") |
| [`ChangingDecimal`](https://docs.manim.community/en/stable/reference/manim.animation.numbers.ChangingDecimal.html#manim.animation.numbers.ChangingDecimal "manim.animation.numbers.ChangingDecimal")                     |

### rotation

与旋转相关的动画。

| 方法名                                                                                                                                                                         | 说明           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ |
| [`Rotate`](https://docs.manim.community/en/stable/reference/manim.animation.rotation.Rotate.html#manim.animation.rotation.Rotate "manim.animation.rotation.Rotate")         | 旋转一个 Mobject |
| [`Rotating`](https://docs.manim.community/en/stable/reference/manim.animation.rotation.Rotating.html#manim.animation.rotation.Rotating "manim.animation.rotation.Rotating") |              |
#### Rotate

旋转一个`Mobj`。

参数：
- **mobject** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要旋转的 `mobject`。
- **angle**（_浮点数_） – 旋转角度。
- **axis**（_np.ndarray_）– 作为 `numpy` 向量的旋转轴。
- **about_point** ( _Sequence_ [float]|_None_) – 旋转中心。
- **about_edge** ( _Sequence_ [float]_None_ ) – 如果`about_point`为`None`，则此参数指定要作为旋转中心的边界框点的方向。


```python
from manim import *

class UsingRotate(Scene):
    def construct(self):
        self.play(
            Rotate(
                Square(side_length=0.5).shift(UP * 2),
                angle=2*PI,
                about_point=ORIGIN,
                rate_func=linear,
            ),
            Rotate(Square(side_length=0.5), angle=2*PI, rate_func=linear),
            )
```

### specialized

比较特殊的方法，特殊到只有一个。

|                                                                                                                                                                                          |                                                        |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| [`Broadcast`](https://docs.manim.community/en/stable/reference/manim.animation.specialized.Broadcast.html#manim.animation.specialized.Broadcast "manim.animation.specialized.Broadcast") | 从 开始广播一个 mobject `initial_width`，直到达到 `mobject` 的实际大小。 |

#### Broadcast

从 开始广播一个 mobject `initial_width`，直到达到 mobject 的实际大小。

参数：
- **mobject** – 要广播的 `mobject`。
- **focal_point** ( _Sequence_ _[float]_ ) – 广播的中心，默认为 `ORIGIN`。
- **n_mobs** ( _int_ ) – 从焦点出现的 `mobject` 的数量，默认为 `5`。
- **initial_opacity** ( _float_ ) – 从广播发出的 `mobjects` 的起始笔触不透明度，默认为 `1`。
- **final_opacity** ( _float_ ) – 广播发出的 `mobjects` 的最终笔触不透明度，默认为 `0`。
- **initial_width** ( _float_ ) – `mobjects` 的初始宽度，默认为 `0.0`。
- **remover**（_bool_）-动画结束后是否应从场景中移除 `mobjects`，默认为 `True`。
- **lag_ratio** ( _float_ ) – `mobject` 每次迭代之间的时间，默认为 `0.2`。
- **run_time** ( _float_ ) – 动画的总持续时间，默认为 `3`。
- **kwargs**（_任意_） – 要传递给的附加参数[`LaggedStart`](https://docs.manim.community/en/stable/reference/manim.animation.composition.LaggedStart.html#manim.animation.composition.LaggedStart "manim.animation.composition.LaggedStart")。



参数很多，代码倒是简单（~~纸老虎bushi~~）：

```python
from manim import *

class BroadcastExample(Scene):
    def construct(self):
        mob = Circle(radius=4, color=TEAL_A)
        self.play(Broadcast(mob))
```


### speedmodifier

用于修改动画播放速度的实用程序。

|                                                                                                                                                                                                        |             |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------- |
| [`ChangeSpeed`](https://docs.manim.community/en/stable/reference/manim.animation.speedmodifier.ChangeSpeed.html#manim.animation.speedmodifier.ChangeSpeed "manim.animation.speedmodifier.ChangeSpeed") | 修改已传递动画的速度。 |
> 个人觉得比较麻烦，暂时不想学，请看官方文档。


### transform

动画将一个对象转换为另一个对象。

 请看下方超级目录：

| 方法名                                                                                                                                                                                                                                                                    | 功能                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| [`ApplyComplexFunction`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyComplexFunction.html#manim.animation.transform.ApplyComplexFunction "manim.animation.transform.ApplyComplexFunction")                                         |                                            |
| [`ApplyFunction`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyFunction.html#manim.animation.transform.ApplyFunction "manim.animation.transform.ApplyFunction")                                                                     |                                            |
| [`ApplyMatrix`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyMatrix.html#manim.animation.transform.ApplyMatrix "manim.animation.transform.ApplyMatrix")                                                                             | 将矩阵变换应用于 `mobject`。                        |
| [`ApplyMethod`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyMethod.html#manim.animation.transform.ApplyMethod "manim.animation.transform.ApplyMethod")                                                                             | 通过应用一种方法来为 `mobject` 制作动画。                 |
| [`ApplyPointwiseFunction`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyPointwiseFunction.html#manim.animation.transform.ApplyPointwiseFunction "manim.animation.transform.ApplyPointwiseFunction")                                 | 将逐点函数应用于 `mobject` 的动画。                    |
| [`ApplyPointwiseFunctionToCenter`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyPointwiseFunctionToCenter.html#manim.animation.transform.ApplyPointwiseFunctionToCenter "manim.animation.transform.ApplyPointwiseFunctionToCenter") |                                            |
| [`ClockwiseTransform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ClockwiseTransform.html#manim.animation.transform.ClockwiseTransform "manim.animation.transform.ClockwiseTransform")                                                 | 沿顺时针方向的圆弧变换 `mobject` 的点。                  |
| [`CounterclockwiseTransform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.CounterclockwiseTransform.html#manim.animation.transform.CounterclockwiseTransform "manim.animation.transform.CounterclockTransform")                         | 沿逆时针方向的圆弧变换物体的点。                           |
| [`CyclicReplace`](https://docs.manim.community/en/stable/reference/manim.animation.transform.CyclicReplace.html#manim.animation.transform.CyclicReplace "manim.animation.transform.CyclicReplace")                                                                     | 循环移动物体的动画。                                 |
| [`FadeToColor`](https://docs.manim.community/en/stable/reference/manim.animation.transform.FadeToColor.html#manim.animation.transform.FadeToColor "manim.animation.transform.FadeToColor")                                                                             | 改变对象颜色的动画。                                 |
| [`FadeTransform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.FadeTransform.html#manim.animation.transform.FadeTransform "manim.animation.transform.FadeTransform")                                                                     | 将一个对象淡入另一个对象。                              |
| [`FadeTransformPieces`](https://docs.manim.community/en/stable/reference/manim.animation.transform.FadeTransformPieces.html#manim.animation.transform.FadeTransformPieces "manim.animation.transform.FadeTransformPieces")                                             | 将一个对象的子对象淡入另一个对象的子对象。                      |
| [`MoveToTarget`](https://docs.manim.community/en/stable/reference/manim.animation.transform.MoveToTarget.html#manim.animation.transform.MoveToTarget "manim.animation.transform.MoveToTarget")                                                                         | 将 `mobject` 转换为存储在其`target`属性中的 `mobject`。 |
| [`ReplacementTransform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ReplacementTransform.html#manim.animation.transform.ReplacementTransform "manim.animation.transform.ReplacementTransform")                                         | 将 `mobject` 替换并变形为目标 `mobject`。            |
| [`Restore`](https://docs.manim.community/en/stable/reference/manim.animation.transform.Restore.html#manim.animation.transform.Restore "manim.animation.transform.恢复")                                                                                                  | 将对象转换为其最后保存的状态。                            |
| [`ScaleInPlace`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ScaleInPlace.html#manim.animation.transform.ScaleInPlace "manim.animation.transform.ScaleInPlace")                                                                         | 按特定比例缩放物体的动画。                              |
| [`ShrinkToCenter`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ShrinkToCenter.html#manim.animation.transform.ShrinkToCenter "manim.animation.transform.ShrinkToCenter")                                                                 | 使物体缩小到中心的动画。                               |
| [`Swap`](https://docs.manim.community/en/stable/reference/manim.animation.transform.Swap.html#manim.animation.transform.Swap "manim.animation.transform.Swap")                                                                                                         |                                            |
| [`Transform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.Transform.html#manim.animation.transform.Transform "manim.animation.transform.变换")                                                                                            | `Transform` 将 `Mobject` 转换为目标 `Mobject`。   |
| [`TransformAnimations`](https://docs.manim.community/en/stable/reference/manim.animation.transform.TransformAnimations.html#manim.animation.transform.TransformAnimations "manim.animation.transform.TransformAnimations")                                             |                                            |
| [`TransformFromCopy`](https://docs.manim.community/en/stable/reference/manim.animation.transform.TransformFromCopy.html#manim.animation.transform.TransformFromCopy "manim.animation.transform.TransformFromCopy")                                                     | 执行反向变换                                     |

#### ApplyMatrix

 对`Mobject`应用矩阵变换。
 
参数：
- **matrix**（_np.ndarray_）– 转换矩阵。
- **mobject**（[_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")）– [`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")。
- **about_point** ( _np.ndarray_ ) – 变换的原点。默认为`ORIGIN`。
- **kwargs** – 传递给的进一步关键字参数[`ApplyPointwiseFunction`](https://docs.manim.community/en/stable/reference/manim.animation.transform.ApplyPointwiseFunction.html#manim.animation.transform.ApplyPointwiseFunction "manim.animation.transform.ApplyPointwiseFunction")。



```python
from manim import *

class ApplyMatrixExample(Scene):
    def construct(self):
        matrix = [[1, 1], [0, 2/3]]
        self.play(ApplyMatrix(matrix, Text("Hello World!")), ApplyMatrix(matrix, NumberPlane()))
```

> 由于不懂矩阵变化是啥东东，这里博主看不懂哟。
> 如果有大佬明白欢迎在 Comments 指出原理。

#### ApplyPointwiseFunction

将逐点函数应用于 `mobject` 的动画。



```python
from manim import *

class WarpSquare(Scene):
    def construct(self):
        square = Square()
        self.play(
            ApplyPointwiseFunction(
                lambda point: complex_to_R3(np.exp(R3_to_complex(point))), square
            )
        )
        self.wait()
```

> 看不懂（~~弱~~）。

#### ClockwiseTransform

沿顺时针方向的圆弧变换 `mobject` 的点。


```python
from manim import *

class ClockwiseExample(Scene):
    def construct(self):
        dl, dr = Dot(), Dot()
        sl, sr = Square(), Square()

        VGroup(dl, sl).arrange(DOWN).shift(2*LEFT)
        VGroup(dr, sr).arrange(DOWN).shift(2*RIGHT)

        self.add(dl, dr)
        self.wait()
        self.play(
            ClockwiseTransform(dl, sl),
            Transform(dr, sr)
        )
        self.wait()
```

#### CounterclockwiseTransform

沿逆时针方向的圆弧变换物体的点。

> 你问我参数呢？不知道，文档没写啊～


```python
from manim import *

class CounterclockwiseTransform_vs_Transform(Scene):
    def construct(self):
        # set up the numbers
        c_transform = VGroup(DecimalNumber(number=3.141, num_decimal_places=3), DecimalNumber(number=1.618, num_decimal_places=3))
        text_1 = Text("CounterclockwiseTransform", color=RED)
        c_transform.add(text_1)

        transform = VGroup(DecimalNumber(number=1.618, num_decimal_places=3), DecimalNumber(number=3.141, num_decimal_places=3))
        text_2 = Text("Transform", color=BLUE)
        transform.add(text_2)

        ints = VGroup(c_transform, transform)
        texts = VGroup(text_1, text_2).scale(0.75)
        c_transform.arrange(direction=UP, buff=1)
        transform.arrange(direction=UP, buff=1)

        ints.arrange(buff=2)
        self.add(ints, texts)

        # The mobs move in clockwise direction for ClockwiseTransform()
        self.play(CounterclockwiseTransform(c_transform[0], c_transform[1]))

        # The mobs move straight up for Transform()
        self.play(Transform(transform[0], transform[1]))
```

为什么这么长，问就是官网复制的。

#### CyclicReplace

循环移动物体的动画，看起来像是扑克牌洗牌感觉适合用于制作排序算法的动画。

参数：
- **mobjects** ( [_Mobject_](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") ) – 要转换的 `mobject` 列表。
- **path_arc** ( _float_ ) – 物体到达目标所遵循的弧度角度（以弧度为单位）。
- **kwargs** – 传递给的进一步关键字参数[`Transform`](https://docs.manim.community/en/stable/reference/manim.animation.transform.Transform.html#manim.animation.transform.Transform "manim.animation.transform.变换")。


```python
from manim import *

class CyclicReplaceExample(Scene):
    def construct(self):
        group = VGroup(Square(), Circle(), Triangle(), Star())
        group.arrange(RIGHT)
        self.add(group)

        for _ in range(4):
            self.play(CyclicReplace(*group))
```


#### FadeToColor

改变对象颜色的动画。


```python
from manim import *

class FadeToColorExample(Scene):
    def construct(self):
        self.play(FadeToColor(Text("Hello World!"), color=RED))
```

#### FadeTransform

将一个对象淡入另一个对象。

参数：
- **mobject** – 起始[`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")。
- **target_mobject** – 目标[`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")。
- **stretch**[`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject") – 控制动画过程中目标是否拉伸。默认值： `True`。
- **dim_to_match** – 如果目标对象未自动拉伸，则允许在目标[`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")移入时调整其初始比例。将其分别设置为 `0`、`1` 和 `2`， [`Mobject`](https://docs.manim.community/en/stable/reference/manim.mobject.mobject.Mobject.html#manim.mobject.mobject.Mobject "manim.mobject.mobject.Mobject")分别将目标的长度与 $x$、$y$ 和 $z$ 方向的起始长度相匹配。
- **kwargs** – 进一步的关键字参数被传递给父类。


```python
from manim import *

class DifferentFadeTransforms(Scene):
    def construct(self):
        starts = [Rectangle(width=4, height=1) for _ in range(3)]
        VGroup(*starts).arrange(DOWN, buff=1).shift(3*LEFT)
        targets = [Circle(fill_opacity=1).scale(0.25) for _ in range(3)]
        VGroup(*targets).arrange(DOWN, buff=1).shift(3*RIGHT)

        self.play(*[FadeIn(s) for s in starts])
        self.play(
            FadeTransform(starts[0], targets[0], stretch=True),
            FadeTransform(starts[1], targets[1], stretch=False, dim_to_match=0),
            FadeTransform(starts[2], targets[2], stretch=False, dim_to_match=1)
        )

        self.play(*[FadeOut(mobj) for mobj in self.mobjects])
```


#### FadeTransformPieces

将一个对象的子对象淡入另一个对象的子对象。



```python
from manim import *

class FadeTransformSubmobjects(Scene):
    def construct(self):
        src = VGroup(Square(), Circle().shift(LEFT + UP))
        src.shift(3*LEFT + 2*UP)
        src_copy = src.copy().shift(4*DOWN)

        target = VGroup(Circle(), Triangle().shift(RIGHT + DOWN))
        target.shift(3*RIGHT + 2*UP)
        target_copy = target.copy().shift(4*DOWN)

        self.play(FadeIn(src), FadeIn(src_copy))
        self.play(
            FadeTransform(src, target),
            FadeTransformPieces(src_copy, target_copy)
        )
        self.play(*[FadeOut(mobj) for mobj in self.mobjects])
```
