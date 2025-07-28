Gauss 消元法是高等代数中引入的内容，不过它是很多后续直接法的基础，或许是最直接的直接法

# Gauss 消元

Gauss 消元基于一个很简单的问题，即解线性方程：
$$\begin{cases}
a_{11}x_1+a_{12}x_2+a_{13}x_3+&\cdots~+a_{1n}x_n=b_1\\
a_{21}x_1+a_{22}x_2+a_{23}x_3+&\cdots~+a_{2n}x_n=b_2\\
&\vdots\\
a_{n1}x_1+a_{n2}x_2+a_{n3}x_3+&\cdots~+a_{nn}x_n=b_n\\
\end{cases}$$
改写成矩阵形式为：
$$\begin{bmatrix}
a_{11}& a_{12}& \cdots& a_{1n}\\a_{21}& a_{22}& \cdots& a_{2n}\\& & \vdots& \\a_{n1}& a_{n2}& \cdots& a_{nn}\\
\end{bmatrix}·
\begin{bmatrix}
x_1\\x_2\\\vdots\\a_n
\end{bmatrix}=
\begin{bmatrix}
b_1\\b_2\\\vdots\\b_n
\end{bmatrix}
$$
高等代数里面一般是用 Cramer 法则去做，当为一个完整方阵的时候，不过多数情况下，利用 Cramer 法则过于复杂，因此常常基于初等行变换进行化作前面系数矩阵为上三角形进行处理，也就是：
$$
\begin{bmatrix}
a_{11}& a_{12}& \cdots& a_{1n}&|&b_1\\a_{21}& a_{22}& \cdots& a_{2n}&|&b_2\\& & \vdots&& |&\vdots \\a_{n1}& a_{n2}& \cdots& a_{nn}&|&b_n\\
\end{bmatrix}\to
\begin{bmatrix}
a'_{11}& a'_{12}& \cdots& a'_{1n}&|&b'_1\\
0& a'_{22}& \cdots& a'_{2n}&|&b'_2\\
& & \vdots&& |&\vdots \\
0& 0& \cdots& a'_{nn}&|&b'_n\\
\end{bmatrix}
$$
进而通过由下至上消元则可以得到相关结果

# 列主元消去法

同时，如果有些数值过小，对于整体而言，会增加其舍入误差，因此常见的情况为每次做消元的时候做最大的元，即：
$$\begin{bmatrix}0.001& 2.000& 3.000& 1.000\\
−1.000& 3.712& 4.623& 2.000\\
−2.000& 1.072& 5.643& 3.000\end{bmatrix}\to
\begin{bmatrix}
−1.000& 3.712& 4.623& 2.000\\
−2.000& 1.072& 5.643& 3.000\\
0.001& 2.000& 3.000& 1.000\\\end{bmatrix}$$
全主元素消去法也就是得到了，其核心在于不仅交换行也交换列，但是这种交换操作，基于数据结构所学，其计算量是极其大的。因此会有仅交换行的情况，即列主元交换法：
$$\begin{bmatrix}
a_{11}& a_{12}& \cdots& a_{1n}&|&b_1\\a_{21}& a_{22}& \cdots& a_{2n}&|&b_2\\& & \vdots&& |&\vdots \\a_{n1}& a_{n2}& \cdots& a_{nn}&|&b_n\\
\end{bmatrix}$$
先选取 $\max(a_{1i})=x$，再交换 $x,1$ 行的数据，再做第一次处理，通过消元会有结果：
$$
\begin{bmatrix}
a_{x1}& a_{x2}& \cdots& a_{xn}&|&b_1\\
a_{21}& a_{22}& \cdots& a_{2n}&|&b_2\\
& & \vdots&& |&\vdots \\
a_{11}& a_{12}& \cdots& a_{1n}&|&b_{1}\\
& & 
\vdots&& |&\vdots \\
a_{n1}& a_{n2}& \cdots& a_{nn}&|&b_n\\
\end{bmatrix}\to\begin{bmatrix}
a_{x1}& a_{x2}& \cdots& a_{xn}&|&b_1\\
0& a'_{22}& \cdots& a'_{2n}&|&b'_2\\
& & \vdots&& |&\vdots \\
0& a'_{12}& \cdots& a'_{1n}&|&b'_{1}\\
& & 
\vdots&& |&\vdots \\
0& a'_{n2}& \cdots& a'_{nn}&|&b'_n\\
\end{bmatrix}
$$
然后后面也是重复上述过程，进而迭代



