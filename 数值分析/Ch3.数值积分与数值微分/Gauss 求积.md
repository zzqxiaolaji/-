如果是 Romberg 下的求积公式基本扎根于
# 机械求积

一般来说，形如如下的积分公式，会有*n*次代数精度，这是与幂多项式插值得到的结果一致的：
$$\int^b_af(x)dx=\sum_{i=0}^nA_if(x_i)$$
而其一共有$2n$个参数，但是或许可以通过某种采样，经过调整获得更高的代数精度，就像 Simpson 公式那样。

如果说之前的内容是以插值为蕴涵，那 Gauss 求积则更多关注采样点的构造，即不单纯关注$A_i$，其$x_i$也同样是考量因素。

# Gauss 求积

考虑这个式子：
$$\int_1^0f(x)dx\thickapprox A_0(x_0)+A_1(x_1)$$
由于$4$个参数，至少$2$阶精度，因此尽可能高的代数精度要求的话，以求$3$阶为例：
$$\begin{cases}
A_0+A_1=2\\
A_0x_0+A_1x_1=0\\
A_0x_0^2+A_1x_1^2=\frac{2}{3}\\
A_0x_0^3+A_1x_1^3=0
\end{cases}$$
经过求解可以得到：
$$\begin{cases}
A_0=A_1=1\\
x_0=\frac{\sqrt3}{3}\\
x_0=-\frac{\sqrt3}{3}
\end{cases}$$
这样就会达到三次代数精度的结果，这并不是凑巧，一般来说，$n$个节点可以达到的最高代数精度为$2n-1$次，而能够达到该值的求积法，称之为 Gauss 求积法，而其插值点也被称之为 Gauss 点，形如下式。
$$
\int^b_af(x)\rho(x)dx=\sum_{i=0}^nA_if(x_i)
$$
而高斯点的选取则是为在该权函数下的正交多项式零点，而$n$为给定节点个数
$$\int^b_aw_n(x)f(x)\rho(x)dx=0,w_n(x_i)=0$$
## Gauss-Legendre 求积

Legendre 多项式：
$$\begin{align}
&P_0(x)=1\\
&P_1(x)=x\\
&P_2(x)=(3x^2-1)/2\\
&P_3(x)=(5x^3-3x)/2\\
\end{align}$$
如果取用$n=2$，则会有其解$x=\pm\sqrt3/3$，与上面算的一致

而前面$A_i$的选取则需要求，一般来说，常用的几个：
$$
\int_{-1}^1f(x)dx\thickapprox f(-\frac{\sqrt 3}{3})+f(\frac{\sqrt 3}{3})\thickapprox
 \frac{5}{9}f(-\frac{\sqrt{15}}{5})+\frac{8}{9}f(0)+\frac{5}{9}f(\frac{\sqrt{15}}{5})$$
## Gauss-Chebyshev 求积

Chebyshev 零点：
$$x_k=cos\frac{2k-1}{2n}\pi，共n个零点$$
而其$A_i$的值经过计算，为一个定值，即仅于插值个数有关，为$\pi/n$，即有：
$$\int_{-1}^1f(x)dx\thickapprox\frac\pi n\sum_{i=1}^ncos\frac{2i-1}{2i}\pi$$
[[正交多项式]]
[[函数空间]]
