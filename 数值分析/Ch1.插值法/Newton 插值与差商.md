从Lagrange插值出发，其比较大的问题在于，每次更新迭代都需要相应的更新其基函数，换言之，由于基函数的确定是基于节点个数的。因此在计算速率上，Lagrange多项式的效果非常差。

从一开始导论部分就有说明，数值分析的算法优化一是优化误差，二则是迭代速率。而Newton插值就是Lagrange插值的后者优化。
# 逐次生成

从Lagrange插值除法出发，观察一次的情况：
$$
P_1(x)=\frac{x-f(x_1)}{x_0-x_1}f(x_0)+\frac{x-f(x_0)}{x_1-x_0}f(x_0)
$$
这个很简单，就是初中数学的两点式方程
单纯对于计算而言，两点式也很难计算。对于大多数所用的方法是点斜式，那可以将其转换为：
$$P_1(x)=y_0+\frac{f(x_1)-f(x_0)}{x_1-x_0}(x-x_0)$$
这也就是后面所用的Newton插值思想

同样的基于二阶也可以进行转化，这个涉及到初中数学对构建二次函数的求解，不多赘述：
$$P_2(x)=y_0+\frac{f(x_1)-f(x_0)}{x_1-x_0}(x-x_0)+\frac{\frac{f(x_2)-f(x_0)}{x_2-x_0}-\frac{f(x_1)-f(x_0)}{x_1-x_0}}{x_2-x_0}(x-x_0)(x-x_1)$$
从中观察可以理解到，其迭代公式基本上是由上一个已知节点到引入一个新节点的过度，因此可以加速计算过程。
而这个过程，一般是由差商进行思考进行计算。也就是根据上面的求解信息，可以进行更高次数值的计算。
# 差商

单纯从数值计算的角度去理解差商，对我来说是很难的，这是因为就如教材中所说的，Newton插值法就是Lagrange插值的变形。其实从我的角度来看，这并不是很正确，因为Newton的数学哲学于Lagrange不同，这也造就了这并非单纯数值计算角度较为简单这一引入关键。

Newton的数学哲学，更加聚焦于微积分角度，这也造就了其看问题的角度于Lagrange的纯代数不同，其微积分领域上大成对于其对于这种问题更加喜欢从微积分角度除法。

数学分析的一大关键，在于对导数的理解，而从上面$P_2(x)$的函数构造角度进行观察，可以发现，似乎形如$\frac{f(x_1)-f(x_0)}{x_1-x_0}$的式子，有点像导数的概念。

这似乎与Lagrange插值中，对平均与导数的关系进行进一步理解。这实际上就引出了概念--差商。

**差商**
$$f[x_1,x_0]=\frac{f(x_1)-f(x_0)}{x_1-x_0}$$
回顾一下一阶导数的概念
$$f'(x_0)=\lim_{n\to\frac{1}{\infty}}=\frac{f(x_0+n)-f(x_0)}{n}$$
直观感官上是，差商的概念，是非极限化的导数，同样的，对于高阶数据同样有此关系
$$f[x_2,x_1,x_0]=\frac{f[x_2,x_1]-f[x_1,x_0]}{x_2-x_0}$$
而二阶导数的概念为：
$$f''(x_0)=\lim_{n\to \frac{1}{\infty}}\frac{f'(x_0+n)-f'(x_0)}{n}=\lim_{n\to \frac{1}{\infty}}\frac{f'(x_0+2n)-f'(x_0)}{2n}$$
后面的操作不明朗，或许是由于在离散点数上，通过提取更多点的共性信息，可以得到更加合适值。
变化率的变化率->二阶变化率->因此选用距离差最大的两个前阶
导数的导数->
这也就是从理解上构造了差商概念。而差商的计算依赖最左式与最右式,，与其期间间距。
$$f[x_n,x_{n-1},\cdots,x_1,x_0]=\frac{f[x_n,x_{n-1},\cdots,x_2,x_1]-f[x_{n-1},x_{n-2},\cdots,x_1,x_0]}{x_n-x_0}$$
就像之前所说的逐项生成，这里差商的计算，本质上也是所谓的逐项生产，从下表可以直观看出：

| $f[x_1,x_0]$ |                  |                      |
| ------------ | ---------------- | -------------------- |
| $f[x_1,x_0]$ | $f[x_2,x_1,x_0]$ |                      |
| $f[x_3,x_1]$ | $f[x_3,x_2,x_1]$ | $f[x_3,x_2,x_1,x_0]$ |
# Newton插值

若默认差商为高阶导数的话，类似于泰勒展开式的表述，即有：
$$N_n(x)=f(x_0)+f[x_1,x_0](x-x_0)+\cdots+f[x_n,x_{n-1},\cdots,x_1,x_0](x-x_0)\cdots(x-x_{n-1})$$
更加简练的写法即为：
$$N_n(x)=\sum^n_{i=0}f[x_n,x_{n-1},\cdots,x_0]\prod_{j=0}^{i-1}(x-x_j)$$
（实话说这就是数学的狗屎之处，尤其是数学大牛，因为在他们认为下，这种公式有简洁美，但是在普通人理解下，就是上面的更加易读）

**和Lagrange插值的一致原因**：
本质都是多项式插值，那根据相同节点求出的结果，根据零点唯一性定理，其一定是一致的

**特殊的Newton插值**：
等距差分，这里只讨论前向差分$\bigtriangleup$，后向差分符号为$\bigtriangledown$

一阶差分：
$$\bigtriangleup f(x_i)=f(x_{i+1})-f(x_i)$$
二阶差分：
$$\bigtriangleup^2f(x_i)=[f(x_{i+2})-f(x_{i+1})]-[f(x_{i+1})-f(x_{i})]=f(x_{i+2})-2f(x_{i+1})+f(x_i)$$
则会有等距差分的Newton公式：
定义步长为$h$，等距节点即为：$[x_0,x_0+h,x_0+2h,\cdots,x_0+nh]$则有插值点$x$对于$x_0$相对序号$t$，其表示为$t=(x-x_0)/h$
$$N_n(x)=x_0+\bigtriangleup f(x)t+\bigtriangleup^2f(x)\frac{t(t-1)}{2!}+\cdots+\bigtriangleup^nf(x)\frac{t(t-1)\cdots(t-n+1)}{n!}$$
看上去很像泰勒展开，泰勒展开：
$$f(x)=f(x_0)+f'(x_0)(x-x_0)+f''(x)\frac{(x-x_0)^2}{2!}+\cdots+f^{(n)}(x)\frac{(x-x_0)^n}{n!}$$
这是由于，当将$h\to\frac{1}\infty$的时候，即会有$\bigtriangleup ^nf(x)=hf^{(n)}(x)$，也就导致了$N_n(x)=f(x)$

**Newton 插值公式在等距节点下，就是“离散版”的泰勒展开式**

# 误差

单纯从解得误差的角度来说，直接沿用Lagrange插值的误差的即可，但是实际上，其拥有独立于Lagrange插值的计算公式

Lagrange插值的余项为：
$$\begin{align}
R(x)=f(x)-L(x)=\frac{f^{(n+1)}(\xi)}{n!}w(x)\\
w(x)=(x-x_1)(x-x_2)\cdots(x-x_n)
\end{align}$$
而Newton插值余项则为：
$$R(x)=f(x)-N(x)=f[x,x_n,x_{n-1},\cdots,x_0]w(x)$$
即在更高维度上考量余项增量

# Newton的数学哲学

**Newton 在微积分中的深刻造诣，决定了他以“变化率”为思维核心，主张用导数算子去解析、建模与构造函数。**

Newton 的数学思想深受他在微积分（尤其是导数与变化率）上的重大贡献影响。他对“变化”这一概念的深刻把握，使他的研究方式与构造方法不是问“它在哪”，而是问“它如何变”。

在函数逼近中，他关注的是：函数值如何随 x 的微小变化而变化？

这也就是差商和导数的哲学基础 —— 它们并不是“函数的值”，而是“函数值的变化”

对 Newton 而言，函数不是“静态表”，而是可以作用导数运算的对象。导数成为“揭示系统规律的工具”，这是一种对“微结构”的极度敏感思维模式


[[多项式插值]]
