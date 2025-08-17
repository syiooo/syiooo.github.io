## 1. 基本定义

| 基本函数                                                                                                       | 英文          | 缩写    | 表达式   | 语言描述     |
| ---------------------------------------------------------------------------------------------------------- | ----------- | ----- | ----- | -------- |
| [正弦函数](https://baike.baidu.com/item/%E6%AD%A3%E5%BC%A6%E5%87%BD%E6%95%B0/9601948?fromModule=lemma_inlink)  | *sine*      | $sin$ | $a/c$ | ∠A的对边比斜边 |
| [余弦函数](https://baike.baidu.com/item/%E4%BD%99%E5%BC%A6%E5%87%BD%E6%95%B0/9602078?fromModule=lemma_inlink)  | *cosine*    | $cos$ | $b/c$ | ∠A的邻边比斜边 |
| [正切函数](https://baike.baidu.com/item/%E6%AD%A3%E5%88%87%E5%87%BD%E6%95%B0/10796488?fromModule=lemma_inlink) | *tangent*   | $tan$ | $a/b$ | ∠A的对边比邻边 |
| [余切函数](https://baike.baidu.com/item/%E4%BD%99%E5%88%87%E5%87%BD%E6%95%B0/10798631?fromModule=lemma_inlink) | *cotangent* | $cot$ | $b/a$ | ∠A的邻边比对边 |
| [正割函数](https://baike.baidu.com/item/%E6%AD%A3%E5%89%B2%E5%87%BD%E6%95%B0/10795811?fromModule=lemma_inlink) | *secant*    | $sec$ | $c/b$ | ∠A的斜边比邻边 |
| [余割函数](https://baike.baidu.com/item/%E4%BD%99%E5%89%B2%E5%87%BD%E6%95%B0/10606283?fromModule=lemma_inlink) | *cosecant*  | $csc$ | $c/a$ | ∠A的斜边比对边 |
![上表所对应的图](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507290920936.svg)

> 注：正切函数、余切函数曾被写作tg、ctg，现已不用这种写法。


一个方法**速记**各种关系：

1. 对角乘积为 $1$，即互为倒数。
2. 六边形中任意三个相邻顶点，处于中间的函数等于两边相邻函数值的乘积，例如：$\sin \theta=\cos\theta \tan \theta$
3. 阴影部分三角形，处于上方两个顶点的平方和等于下方顶点的平方值，如：$\sin\theta^{2}+\cos\theta^{2}=1$、$\tan^{2}\theta+1^{2}=\sec ^{2}\theta$ 

![速记方法](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507290937314.svg)

其中倒数关系如下：
- $\sec\theta=\frac{1}{\cos\theta}$ 或 $\sec\theta \cos\theta=1$
- $\csc\theta=\frac{1}{\sin\theta}$ 或 $\csc\theta \sin\theta=1$
- $\cot\theta=\frac{1}{\tan\theta}$ 或 $\cot\theta \tan\theta=1$

商数关系：
- $\tan\alpha=\frac{\sin\alpha}{\cos\alpha}$
- $\cot\alpha=\frac{\cos\alpha}{\sin\alpha}$

平方关系：
- $\sin ^{2}\alpha+\cos ^{2}\alpha=1$
- $1+\cot ^{2}\alpha=\csc ^{2}\alpha$
- $1+\tan ^{2}\alpha=\sec ^{2}\alpha$


## 2. 诱导公式

> $\sin$ 与 $\cos$ 的半个周期加上 $\alpha$ 改变正负号，本质上是因为两者的周期恰好分为正负相反的两个 $\pi$ 周期，故加上半个周期显然是改变符号的。

 $$\sin(\pi+\alpha)=-\sin\alpha$$
 $$\cos(\pi+\alpha)=-\cos\alpha$$

> $\frac{\pi}{2}\pm\alpha$ 的关系

1. $sin$

$$\sin \frac{\pi}{2}\pm\alpha=\cos\alpha$$

2. $cos$
$$
\cos \frac{\pi}{2}+\alpha=-\sin\alpha
$$
$$
\cos \frac{\pi}{2}-\alpha=\sin\alpha
$$
3. $tan$
$$
\tan \frac{\pi}{2}+\alpha=-\cot\alpha
$$
$$
\tan \frac{\pi}{2}-\alpha=\cot\alpha
$$
4. $cot$
$$
\cot \frac{\pi}{2}+\alpha=-\tan\alpha
$$
$$
\cot \frac{\pi}{2}-\alpha=\tan\alpha
$$


## 3. 函数图像

### 3.1 正弦

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507291021523.png)

### 3.2 余弦


![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507291021923.png)

### 3.3 正切

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507291022946.png)

### 3.4 余切

![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507291022761.png)
### 2.3 正割
![](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202507291022185.png)

### 3.6 余割
![image.png](https://ccccooh.oss-cn-hangzhou.aliyuncs.com/img/202508180232226.png)

## 4. 三角函数常用公式

### 4.1 倍角公式

1. $\sin 2\alpha$ = $2\sin\alpha \cos\alpha$
2. $\cos2\alpha$ = $\cos ^{2}\alpha-\sin ^{2}\alpha$ = $1-2\sin ^{2}\alpha$ = $2\cos ^{2}\alpha-1$
3. $\sin3\alpha$ = $-4\sin ^{3}\alpha+3\sin\alpha$
4. $\cos3\alpha$ = $4\cos ^{3}\alpha-3\cos\alpha$
5. $\tan 2\alpha$ = $\frac{2\tan\alpha}{1-\tan ^{2}\alpha}$
6. $\cot2\alpha$ = $\frac{{\cot ^{2}\alpha-1}}{2\cot\alpha}$

### 4.2 半角公式

1. $\sin \frac{^{2}\alpha}{2}$ = $\frac{1}{2}(1-\cos\alpha)$
2. $\cos \frac{^{2}\alpha}{2}$ = $\frac{1}{2}(1+\cos\alpha)$
3. $\sin \frac{\alpha}{2}$ = $\pm \sqrt{ \frac{{1-\cos\alpha}}{2} }$
4. $\cos \frac{\alpha}{2}$ = $\pm \sqrt{ \frac{1+\cos\alpha}{2} }$
5. $\tan \frac{\alpha}{2}$ = $\frac{{1-\cos \alpha}}{\sin\alpha}$ = $\frac{\sin\alpha}{1+\cos\alpha}$ = $\pm \sqrt{ \frac{{1-\cos\alpha}}{1+\cos\alpha} }$
6. $\cot \frac{\alpha}{2}$ = $\frac{\sin\alpha}{{1-\cos \alpha}}$ = $\frac{1+\cos\alpha}{\sin\alpha}$ = $\pm \sqrt{ \frac{1+\cos\alpha}{{1-\cos\alpha}} }$

### 4.3 和差公式

1. $\sin(\alpha\pm\beta)$ = $\sin\alpha \cos\beta\pm \cos\alpha \sin\beta$
2. $\cos(\alpha\pm\beta)$ = $\cos\alpha \cos\beta\mp \sin\alpha \sin\beta$
3. $\tan(\alpha\pm\beta)$ = $\frac{{\tan\alpha\pm \tan\beta}}{1\mp \tan\alpha \tan\beta}$
4. $\cot(\alpha\pm\beta)$ = $\frac{{\cot\alpha \cot\beta\mp{1}}}{\cot\beta\pm \cot\alpha}$

### 4.4 积化和差与和差化积公式

1️⃣积化和差公式
1.  $\sin\alpha \cos\beta$ = $\frac{1}{2}[\sin(\alpha+\beta)+\sin(\alpha-\beta)]$
2. $\cos\alpha \sin\beta$ = $\frac{1}{2}[\sin(\alpha+\beta)-\sin(\alpha-\beta)]$
3. $\cos\alpha \cos\beta$ = $\frac{1}{2}[\cos(\alpha+\beta)+\cos(\alpha-\beta)]$
4. $\sin\alpha \sin\beta$ = $-\frac{1}{2}[\cos(\alpha+\beta)-\cos(\alpha-\beta)]$

简化口诀：

> $S表示\sin,C表示\cos$，左边依次填入 $\alpha,\beta$，右边依次填入 $\alpha+\beta,\alpha-\beta$.


1. $SC$ = $\frac{1}{2}(S+S)$
2. $CS$ = $\frac{1}{2}(S-S)$
3. $CC$ = $\frac{1}{2}(C+C)$
4. $SS$ = $-\frac{1}{2}(C-C)$



2️⃣和差化积公式 (本质就是从积化和差换元得到的)
1. $\sin\alpha+\sin\beta$ = $2\sin \frac {{\alpha+\beta}} {2} \cos \frac{{\alpha-\beta}}{2}$
2. $\sin\alpha-\sin\beta$ = $2\sin \frac {{\alpha-\beta}} {2}\cos \frac{{\alpha+\beta}}{2}$
3. $\cos\alpha+\cos\beta$ = $2\cos \frac {{\alpha+\beta}} {2} \cos \frac{{\alpha-\beta}}{2}$
4. $\cos\alpha-\cos\beta$ = $-2\sin \frac {{\alpha+\beta}} {2} \sin \frac{{\alpha-\beta}}{2}$


### 4.5 万能公式

若 $u=\tan \frac{x}{2} (-\pi<x<\pi)$，则 $\sin x= \frac{2u}{1+u^{2}}$，$\cos x = \frac{{1-u^{2}}}{1+u^{2}}$.


## 5. 反三角恒等式

> 在第九讲中计算题的一个步骤里用到了，知识到用时方恨少，记得有这么一个公式 $\sin(\arccos x)=\dots$ 却怎么也想不起来...好在最好找到了，在第一讲 P 69，为了避免下次再忘记，整理在这里并导入 Anki 背诵。

1. $\sin(\arcsin x)$ = $x, x\in[-1,1]$
2. $\sin(\arccos x)$ = $\sqrt{ 1-x^{2} }, x\in[-1,1]$
3. $\cos(\arccos x)$ = $x,x\in[-1,1]$
4. $\cos(\arcsin x)$ = $\sqrt{ 1-x^{2} },x\in[-1,1]$
5. $\arcsin(\sin x)$ = $x,x\in\left[ - \frac{\pi}{2}, \frac{\pi}{2} \right]$
6. $\arccos(\cos x)$ = $x,x\in[0, \pi]$
7. $\arcsin x+\arccos x$ = $\frac{\pi}{2}(-1\leq x\leq 1)$

## 6. 诱导公式

> 诱导公式，极其重要。掌握主动配凑诱导公式才能解决部分换元法的题目，不能只停留在计算诱导公式的层面，更多要学会逆用和主动配凑。
> 见三十讲 P 597 附录.

下面八个公式要熟稔于心，常用于换元法当中：

1. $\sin\left( \frac{\pi}{2}\pm\alpha \right)$ = $\cos\alpha$
2. $\sin(\pi\pm\alpha)$ = $\mp \sin\alpha$
3. $\cos\left( \frac{\pi}{2}\pm\alpha \right)$ = $\mp \sin\alpha$
4. $\cos(\pi\pm\alpha)$ = $-\cos\alpha$

