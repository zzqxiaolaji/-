关于为什么要学正交变换以及为什么需要学这两种典型的正交变换呢，这是因为一个很有趣的点：
$$A^{-1}=A^T$$
也就是说我可以正向的取求解 $A$ 然后通过直接取其转置矩阵，然后可以达成不需要求逆矩阵的效果。这个看似很多余，为此我甚至线下取询问过课程老师，既然是都是做线性变换，为什么不能用类似 Gauss 消元法的初等变换去做 QR 分解，不过后来想的就是：
$$HA=U\to A=H^{-1}U=QR$$
那么求解 $H^{-1}$ 又成了一个问题，而利用正交矩阵则不会有这样的问题。

# Householder 变换

Householder 变换实际上做了这么一件事情，把一个向量通过正交变换使得其可以映射到任何一个平面上，也就是做到了相应的线性变换。

考虑这么一个向量 $\omega=(\omega_1,\omega_2,\cdots,\omega_n)^T$ ，那么就会有在这个单位向量上的 Householder 变换：
$$H(\omega)=I-2\omega\omega^T$$
他就会以 $\omega$ 为超平面的法向量，将任意一个向量 $v$ 化作 $\omega$  和超平面上的两个分量，对 $\omega$ 上的做逆转，而对超平面的不做改变。那么就会有效果，从视觉角度来看就是“镜面反射”。

这个看似没什么大用，实际上其达到了一件事，即只要确定好了单位向量 $\omega$ 就可以通过“镜面反射”的方式导致任一向量投影到我们希望的方向上，不过方向不变。实际上这个思想和初高中找角平分线的一致。

那拥有了这个变换之后，就可以做到相应的一些线性变换操作，即：
$$HA=A'\Rightarrow A=H^TA'$$
拥有了 $A,A'$ 就是找相应的 Householder 变换了，这就是约化定理的内容了。

# Householder 约化

在 LU 变换里面，通常都喜欢做这这样的操作以致使得到一个上三角矩阵：
$$\begin{cases}
l_{22}·u_{22}+l_{21}u_{12}=a_{22}\\
l_{22}·u_{23}+l_{21}u_{13}=a_{23}\\
l_{22}·u_{24}+l_{21}u_{14}=a_{24}\\
~~~~~\cdots\\
l_{22}·u_{2n}+l_{2n}u_{1n}=a_{2n}\\
\end{cases}\to
U=\begin{bmatrix}
a_{11}& a_{12}& \cdots& a_{1n}& \\
& a_{22}-l_{21}u_{13}& \cdots& a_{2n}-l_{21}u_{1n}&\\
& & \ddots& \vdots\\
& & & u_{nn}
\end{bmatrix}$$
实际上 Householder 的情景也类似，不过是希望通过正交变换去得到一个上三角矩阵，那么其小迭代过程中所得到的就是，每次把一个列向量约化成基向量上的单位向量乘以其范数大小。

这边先解释一下为什么可以把一个列向量约化成基向量上的单位向量乘以其范数大小，也就是：
$$||x||_2=||y||_2\Rightarrow \exists H,s.t.Hx=y$$
那么就会有 Householder 约化：
$$已有信息：向量~x，想要约化到单位向量~e_1~上面$$
那么基本步骤就是：
$$||x||_2=\sigma$$
进而得到平面法向量 $v$ ：
$$v=x+||x||_2e_1=x+\sigma e_1$$
不过这个不适用于一开始说的 Householder 变换：
$$w=\frac{v}{||v||}$$
那么就会有：
$$H(w)=I-2ww^T$$
# Householder QR

对于一个矩阵，这里以方阵为例：
$$\begin{bmatrix}
a_{11}&a_{12}&\cdots&a_{1n}\\
a_{21}&a_{22}&\cdots&a_{2n}\\
&&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}\\
\end{bmatrix}$$
step1：$$x_1=[a_{11},a_{21},\cdots,a_{n1}]^T$$
基于这个 $x_1$ 与 $[1,0,\cdots,0]^T=e_1$ 去找到  $H(\omega_1)$
$$H(\omega_1)\begin{bmatrix}
a_{11}&a_{12}&\cdots&a_{1n}\\
a_{21}&a_{22}&\cdots&a_{2n}\\
&&\vdots\\
a_{n1}&a_{n2}&\cdots&a_{nn}\\
\end{bmatrix}=\begin{bmatrix}
\sigma_1&a'_{12}&\cdots&a'_{1n}\\
0&a'_{22}&\cdots&a'_{2n}\\
&&\vdots\\
0&a'_{n2}&\cdots&a'_{nn}\\
\end{bmatrix}$$
step2：
$$\begin{bmatrix}
\sigma_1&a'_{12}&\cdots&a'_{1n}\\
0&a'_{22}&\cdots&a'_{2n}\\
&&\vdots\\
0&a'_{n2}&\cdots&a'_{nn}\\
\end{bmatrix}$$
$$x_2=[a_{22}',a_{23}',\cdots,a_{2n}']^T$$
同样基于这个去找到 $\overline{H_{2}}$ ，但是值得注意的是，这个 $\overline{H}(\omega_{2})$ 的大小为 $n-1\times n-1$ 
那么就需要真正的
$$H(\omega_2)=\begin{bmatrix}1& 0\\0&\overline{H}(\omega_{2})\end{bmatrix}$$
注意这里的 $0$ 是零向量
后面可以得到：
$$H(\omega_2)\begin{bmatrix}
\sigma_1&a'_{12}&\cdots&a'_{1n}\\
0&a'_{22}&\cdots&a'_{2n}\\
&&\vdots\\
0&a'_{n2}&\cdots&a'_{nn}\\
\end{bmatrix}=\begin{bmatrix}
\sigma_1&a'_{12}&\cdots&a''_{1n}\\
0&\sigma_2&\cdots&a''_{2n}\\
&&\vdots\\
0&0&\cdots&a''_{nn}\\
\end{bmatrix}$$
后面步骤一致，这样就可以把其化作一个上三角矩阵
[[常见算法思想]]