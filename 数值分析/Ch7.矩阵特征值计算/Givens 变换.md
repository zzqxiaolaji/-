与 Householder 变换一致，都是正交变换。
不过这个和二维空间内的旋转变换一致：
$$P(\theta)=\begin{bmatrix}\cos\theta& \sin\theta\\-\sin\theta& \cos\theta\end{bmatrix}$$
这个变换会导致其依照原点进行旋转，而实际上 Givens 变换做的就说这样的旋转，不过这里的 Givens 变换是：
$$P(i,j,\theta)=\begin{bmatrix}
1\\
& \ddots\\
&& \cos\theta& &\cdots& & \sin\theta\\
&&&1\\
&&\vdots&&\ddots&&\vdots\\
&&&&&1\\
&& -\sin\theta& &\cdots& & \cos\theta\\
&&&&&&&1\\
&&&&&&&&\ddots\\
&&&&&&&&&1
\end{bmatrix}$$
与 Householder 变换一样，其同样可以把其转变为一个目的向量。
[[Householder 变换]]
