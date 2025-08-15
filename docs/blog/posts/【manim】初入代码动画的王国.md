---
title: 【manim】初入代码动画的王国
author: Sy
cover: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
thumbnail: 'https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/hollow.jpg'
index_img: >-
  https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-10%2016.58.38.png
sticky: 0
expires: ''
excerpt: manim 官方演示代码笔记。
tags:
  - manim
categories:
  - manim
date: 2025-07-10 17:00:00
---
## 介绍

在使用 `Manim` 之前，要先知道 Manim 分为很多个版本：

- Manim Community - 开发者维护版，也就是社区版
- Manim master - 作者维护的版本
- Manim-cairo-backend 不维护的旧版

开源社区的力量是不可忽视的，我选择社区版。

> 注意社区版的代码风格和作者本人维护的版本有一定区别。

<!-- more -->

## 部署 Manim 环境

一开始我尝试了直接在 PycharmCE 中配置软件包来使用 Manim，总得来说效果不太好，还是老老实实按照文档安装来的方便。

详细步骤建议根据 [Manim Community 官网文档](https://docs.manim.community/en/stable/installation.html)来，本文中贴出关键步骤：

> 以 MacOS 为例

1. 安装 `uv` 环境：

```bash
curl -LsSf <https://astral.sh/uv/install.sh> | sh
```

2. 利用 `uv` 工具安装 `python` ：

```bash
uv python install
```

3. `Latex` 环境的安装为optional，官网未进行详细说明以文档为准。
4. 建立一个文件夹作为项目目录，进入项目根目录后执行：

```bash
uv init manimations
cd manimations
uv add manim
```

5. 检测 manim 环境：

```bash
uv run manim checkhealth
```

> 如果弹出视频则证明成功。

- **Tip**：⚠️我在阅读官方文档前胡乱安装了很多库，故确保以上步骤执行后可能缺少部分依赖，具体以文档为准。

## 渲染命令

学习 Manim 最快的方式我认为是直接读源码示例，直接上一段代码后我们来进行拆分。manim 的代码结构非常简单易懂。

打开 `Vscode` ，在方才我们建立的 `manimations` 目录中有一个 `main.py` 文件，写入：

```python
from manim import *

class SingleScene(Scene):
    def transform(self):
        a = Circle()
        b = Square()
        c = Triangle()
        self.play(Transform(a, b))
        self.play(Transform(a, c))
        self.play(FadeOut(a))
        
    def construct(self):
        self.transform()
        self.wait(0.5)  # wait for 0.5 seconds
```


然后在终端中输入：

```bash
manim -pqm main.py SingleScene
```

讲讲终端中的命令做了什么，`manim` 是我们的工具名，对`main.py`下的`SingleScene`这个场景类进行了渲染，其中`-pqm`的说明可以使用`manim render --help`来查看：

```bash
-p, --preview                  Preview the Scene's animation. OpenGL does a

-q, --quality [l|m|h|p|k]      Render quality at the follow resolution
                                 framerates, respectively: 854x480 15FPS,
                                 1280x720 30FPS, 1920x1080 60FPS, 2560x1440
                                 60FPS, 3840x2160 60FPS
```

那么很明显了，`p`表示预览视频，`q`表示视频质量，你可以用`l`来加快速度，但我更倾向于`m`所带来的体验。

执行后会弹出渲染出的视频。

![效果](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-10%2016.35.10.png)

> 接下来为了省事就不把截图贴出来了。

在深入挖掘 manim 的官方文档后我发现了一种更加省事儿的方法，无需操作命令行参数：
```python
from manim import *
class SingleScene(Scene):
  ...

# 使用 temconfig 方法，调用 scene.render()
with tempconfig({"quality": "medium_quality", "preview": True}):
    scene = SingleScene()
    scene.render()
```

`tempconfig`是 manim 自带的上下文资源管理器，`with`语句用于正常释放资源，效果等价于`try`语句。


## 语法

上文说完了`manim`的渲染方法，以及文件的执行方法。下面来扒一下这段代码是在干什么。

```python
from manim import *

class SingleScene(Scene):
    def transform(self):
        a = Circle()
        b = Square()
        c = Triangle()
        self.play(Transform(a, b))
        self.play(Transform(a, c))
        self.play(FadeOut(a))
        
    def construct(self):
        self.transform()
        self.wait(0.5)  # wait for 0.5 seconds
```

第一步导入了库函数就不多说了吧。接着我们定义了一个场景类，接着定义了一个函数（实际上这个函数并不重要，重要的是`construct`中的代码。

> `construct`就是 `manim` 中的程序入口，执行渲染是从这里开始渲染。

也就是相当于我们在`construct`中写了这样的代码：
```python
a = Circle()
b = Square()
c = Triangle()

self.play(Transform(a, b))
self.play(Transform(a, c))
self.play(FadeOut(a))
self.wait(0.5)  # wait for 0.5 seconds
```

我们利用`Cricle()`、`Square()`, `Triangle()` 分别创建了一个圆、正方形、三角形。

这段代码实际上是就是定义了三个“演员”，在这里预设好演员的长相，位置等等属性和参数。然后利用`self.play()`将演员搬上舞台，并且按照顺序表演。

其实就这么 easy，下面来看点常见的函数。

## 定义方法

用我自己的话说，这叫定义“演员”的属性，其实和游戏开发中很多思想一致。在实际表演的过程中再去动态改变这个属性即可达到动画的效果。

> 当然了，官方可不是这么定义的（~~bushi~~ 演员）。

```python
a = Circle() # 定义一个圆
b = Square() # 方
c = Triangle() # 三角
square = Square(color=BLUE, fill_opacity=.6).shift(2 * LEFT) # 颜色、透明度、移动（左为例）
square.set_fill(BLUE, opacity=0.2) # 设置方的实心填充，颜色、透明度
circle.next_to(square) # 设置圆的位置为临近于方

```

有点过于简单了，毕竟这是新手村嘛。

## 动画方法

动画方法也是我自己起的名字，官方应该不这么叫吧。上文也提及了，这一类方法就是定义动画是怎么“动态”地执行的，基本上就是写在`self.play()`里面。也就是说场景类才是剧本的总指挥官。

```python
self.play(Transform(a, b)) # 让a转换到 b
self.wait(0.5) # 停留一会儿，参数可以缺省
self.play(FadeIn(square)) # 让square 淡入场景，有淡入必有淡出：FadeOut()
self.play(Create(circle)) # 动态构建出圆
self.play(ReplacementTransform(square, circle)) # 一种特殊的转换，与Transform 实测差别不大，具体取决于习惯
self.play(square.animate.rotate(PI / 2)) # 让方旋转
self.play(left_square.animate.rotate(PI), Rotate(right_square, angle=PI, run_time=2)) # 官方演示中的进阶方法，同时操作两个对象，同时设置运行 duration
```

新手村就到这里，你可以出村了少年。

最后回顾一下，其实无非就是这样的结构：
```python
from manim import *

class SingleScene(Scene):
	def construct(self):
		定义
		执行
```

## 杂谈

实际上你还可以使用`Vscode`的扩展来动态渲染。不过经过我的实际体验，效果不是很好。每次保存都会自动 render 一遍，特别烦。

![Manim Sideview](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/%E6%88%AA%E5%B1%8F2025-07-10%2016.58.38.png)