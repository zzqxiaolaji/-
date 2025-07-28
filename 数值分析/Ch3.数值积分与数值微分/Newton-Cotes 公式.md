本质基于插值
# Simpson 公式

在梯形公式的基础上，若我们进一步思考：如果函数在 $[a,b]$ 区间内不是线性变化，而更接近**抛物线**（即二次函数）呢？我们便可用一个**通过三点的抛物线**来逼近函数图像，这样得到的就是**Simpson 公式**。

我们取 $a$、$b$ 及中点 $m = \frac{a + b}{2}$ 三点，用抛物线插值它们，从而得到积分近似：

取三点$a,\frac{a+b}2,b$，记其函数值分别为$f_0,f_1,f_2$，通过 Lagrange 插值进行二次上的拟合，则会有：
$$\begin{align}L_0(x)=\frac{(x-x_1)(x-x_2)}{(x_0-x_1)(x_0-x_2)}&=\frac{(x-a-(b-a)/2)(x-a-(b-a))}{(b-a)^2/2}\\
&=\frac{(2x-2a-b+a)(x-a-b+a)}{(b-a)^2}\\
&=\frac{(2x-a-b)(x-b)}{(b-a)^2}
\end{align}$$
基于这个做积分运算（很复杂，叫 gpt 辅助证明了也要半天）会有：
$$\int_a^bL_0(x)dx=\frac{b-a}6$$
同样经过上述操作会有：
$$\begin{align}
\int_a^bL_1(x)dx&=\frac{4(b-a)}6\\
\int_a^bL_2(x)dx&=\frac{(b-a)}6
\end{align}$$
通过乘以相应基函数的值则会有：
$$
\int_a^b f(x)\,dx \approx \frac{b-a}{6}[f(a) + 4f(\tfrac{a + b}{2}) + f(b)]
$$
# Cotes 系数

Newton-Cotes 公式，由两部分，关于 Newton 的解读，在于使用幂函数进行插值计算，也就是利用Lagrange 插值对 Simpson 公式构造的方式一致。而 Cotes  则在于归纳总结积分结果，即得到了Cotes 系数。

积分区间$[a,b]$划分为$n$等分，步长为$\frac{b-a}{n}$，选取等距节点$x_k=a+kh$进行插值型求积：
$$I_n=(b-a)\sum_{k=0}^nC_n^{(k)}f(x_k)$$
其中 Cotes 系数为：
$$C_n^{(k)}=\frac{h}{b-a}\int^n_0\prod_{j=0,j\neq k}^n\frac{t-j}{k-j}dt=\frac{(-1)^{n-k}}{nk!(n-k)!}\prod_{j=0,j\neq k}^n{(t-j)dt}$$
以四次 Cotes 为例计算
$$\begin{align}
C_4^{(0)}=\frac{(-1)^4}{4*0!*(4-0)!}\int^4_0(t-1)(t-2)(t-3)(t-4)dt=\frac7{90}
\end{align}
$$
$n=2$ 时，Simpson 公式
$$
S(x)=\frac{b-a}{6}(f(a)+4·f(a+\frac{b-a}{2})+f(b))
$$
$n=4$ 时，Cotes 公式
$$
C(x)=\frac{b-a}{90}(7f(a)+32f(a+\frac{b-a}{4})+12f(a+\frac{2(b-a)}{4})+32f(a+\frac{3(b-a)}{4})+7f(a))$$
# 余项

作为$n$次幂多项式得到的插值结果进行的积分，Newton-Cotes 公式拥有至少$n$次代数精度。但是以 Simpson 公式为例，当 $n=3$ 时，有：
$$S=\frac{b-a}{6}[a^3+4(\frac{a+b}{2})^3+b^3=\frac{b^4-a^4}{4}$$
故其结果一致，即有$3$次代数精度。

这很难理解，因为我们是对 $n=2$ 的时候做的这个，但是却拥有了超收敛的情景。这是因为：如果将 $(b-a)/2$ 作为原点的话，其左右积分区间为对称，因此对于奇数项的$x^n$，会有$\int^b_ax^ndx=0$

所以有此结论：当阶数 $n$ 为偶数时，其代数精度为 $n+1$ 

余项的提出则基于其代数精度进行求解，一般为根据具体情境求解，其基于一般的余项公式与刚才所求的公式在其最高代数精度+1次的维度求解
$$\begin{align}
&R[f]=Kf^{n}(\eta)\\
&K=\frac{1}{(n+1)!}[\frac{1}{n+2}(b^{n+2}-a^{n+2})-(b-a)\sum_{k=0}^nC_n^{(k)}x_k^{n+1}]\\
\\
&n:Newton-Cotes ~的最高代数精度
\end{align}$$
以 Simpson 公式为例：
$$R_2[f]=\frac{1}{4!}[\frac{1}{5}(b^5-a^5)-\frac{b-a}{6}(a^4+(\frac{a+b}{2})^4+b^4)]f^{(4)}(\eta)=\frac{1}{24}\frac{(b-a)^5}{120}=\frac{b-a}{180}(\frac{b-a}{2}^4)f^{(4)}(\eta)$$

**常见的几种 Newton-Cotes 公式与余项**

公式：
$$\begin{align}
&n=1~时~~T=\frac{b-a}{2}[f(a)+f(b)]\\
&n=2~时~~S=\frac{b-a}{6}[f(a)+4f(\frac{a+b}{2})+f(b)]\\
&n=4~时~~C=\frac{b-a}{90}(7f(a)+32f(a+\frac{b-a}{4})+12f(a+\frac{2(b-a)}{4})+32f(a+\frac{3(b-a)}{4})+7f(a))

\end{align}$$
余项：
$$\begin{align}
&n=1~时~~R[T]=\frac{b-a}{12}(\frac{b-a}{2})^2f'(\xi)\\
&n=2~时~~R[S]=-\frac{b-a}{180}(\frac{b-a}{2})^4f'(\xi)\\
\end{align}$$
[[矩形公式与梯形公式]]
[[常见算法思想]]


