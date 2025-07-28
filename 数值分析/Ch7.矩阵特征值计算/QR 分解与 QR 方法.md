# QR 分解

任何一个矩阵 $A$  都可以变成一个正交矩阵 $Q$  与上三角矩阵 $R$ 的乘积：
$$A=QR$$
这里面常常使用 Householder 和 Givens 变换使得其达到相应的 $Q$ 的求解。

# 约化为 Heisenberg 矩阵

一般的矩阵可以通过 QR 分解手段去得到一个上三角矩阵，那么实际上对于需要约化成 Heisenberg 的情况也可以做相关操作：
基于 Householder 做 QR 分解的操作，也会有：
$$\begin{bmatrix}
a_{11}&a_{12}&\cdots&a_{1n}\\
a_{21}&a_{22}&\cdots&a_{2n}\\
&&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}\\
\end{bmatrix}$$
step1：
$$x_1=[a_{21,}a_{23},\cdots,a_{n1}]^T$$
$$H(\omega_1)=\begin{bmatrix}1& 0\\0&\overline{H}(\omega_{1})\end{bmatrix}$$
这里与传统的 QR 分解就不同了，这里需要做 $H(\omega_1)AH(\omega_1)$，而 QR 仅需要，$H(\omega_1)A$
$$H(\omega_1)AH(\omega_1)=\begin{bmatrix}
a_{11}&a'_{12}&\cdots&a'_{1n}\\
\sigma_1&a'_{22}&\cdots&a'_{2n}\\
&&\vdots\\
0&a'_{n2}&\cdots&a'_{nn}\\
\end{bmatrix}$$
后面的步骤和上述一致，只不过需要忽略多一行。

最后会有：
$$H_{n-1}\cdots H_2H_1AH_1H_2\cdots H_{n-1}=Heisenberg$$
值得注意的是，如果 $A$ 是一个对称矩阵的话，这么做会导致其为一个三对角矩阵。

# QR 方法

对于 QR 分解有：
$$A=QR\Rightarrow B=RQ=Q^TAQ$$
那么也就是 $A$ 和 $B$ 做了一个相似变换（正交矩阵），那么其特征值也就不会边，而这个过程会逐步使得其分量线性变换的结果趋于自身，也就是在对角上展现特征值，即：
$$\lim_{k\to\infty}A_k\Rightarrow R=\begin{bmatrix}\lambda_1& *& \cdots&*\\&\lambda_2& \cdots&*\\&&\ddots&\vdots\\&&&\lambda_n\end{bmatrix}$$
那么 QR 方法的迭代过程就是：
$$\begin{align}
&A_0=Q_0R_0\\
&A_1=R_0Q_0=Q_1R_1\\
&A_2=R_1Q_1=Q_2R_2\\
&\cdots\\
&A_k=R_{k-1}Q_{k-1}=Q_kR_k\\
\end{align}$$
也就是逐渐提升其上三角程度，有点像分离的幂法
[[幂法与反幂法]]
[[Householder 变换]]
[[Givens 变换]]