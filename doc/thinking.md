# 思路参考
几个简单的就不给参考了，这里只提供行列式、求逆和秩的实现思路
## 行列式求解
对于一个 $n \times n$ 的方阵 $\textbf{A}$ ，由Laplace定理可知：

$$|\textbf{A}|=\sum_{j=1}^n (-1)^{i+j}a_{ij}|\textbf{A}_{ij}|$$

其中 $\textbf{A}_{ij}$ 表示矩阵 $\textbf{A}$ 将第 $i$ 行和第 $j$ 列删除后得到的子矩阵。因此可以通过递归的方式进行求解，其中递归结束为 $n=2$ 的情形，即

$$
\begin{vmatrix}
a_{11} & a_{12} \\ 
a_{21} & a_{22}
\end{vmatrix}
=a_{11}a_{22}-a_{12}a_{21}
$$

对于 $n=1$ 的情形，直接返回该元素本身即可。
## 逆矩阵求解
对于一个 $n\times n$ 的方阵 $\textbf{A}$ ，应首先判断其是否可逆，矩阵 $\textbf{A}$ 可逆的充要条件为

$$|\textbf{A}|\neq 0$$

如果方阵是可逆的，则可以根据伴随矩阵来求逆，即

$$ \textbf{A}^{-1}=\dfrac{1}{|\textbf{A}|}\textbf{A}^* $$

其中 $\textbf{A}^*$ 是 $\textbf{A}$ 的伴随矩阵，其计算方法为：

$$a^*_{ij}=(-1)^{i+j}|\textbf{A}_{ji}|$$

与前一部分相同， $\textbf{A}_{ji}$ 表示矩阵 $\textbf{A}$ 将第 $j$ 行和第 $i$ 列删除后得到的子矩阵。
因此只需要构造出伴随矩阵，即可完成求逆运算。
## 秩的求解
求解矩阵的秩可以通过高斯消元法转化为上三角矩阵的过程来实现。对于 $m\times n$ 的矩阵 $\textbf{A}$ ，求 $\text{rank}(\textbf{A})$ 的具体步骤为：

1. 首先，函数初始化秩为矩阵行数和列数中较小的那个值：

   $$ \text{rank} = \min(m,n) $$

2. 接下来，函数将矩阵转化为上三角形式。这个过程通过高斯消元法实现，具体步骤如下：

   a. 对于每一列 $i$，从第 $i$ 行开始，检查当前主对角线上的元素 $a_{ii}$ 是否为零。如果 $a_{ii} \neq 0$ ，则表示可以进行高斯消元操作，否则需要在下方寻找一个非零元素，并将其与当前行交换。

   b. 对于当前主对角线上的元素 $a_{ii}$，将其下方的所有元素通过行运算消除为零，使得当前列下方的元素全部为零。

   c. 如果主对角线上的元素 $a_{ii}$ 为零，则需要在下方寻找一个非零元素 $a_{ji}$ 并将其与当前行交换，以确保在进行下一轮高斯消元时可以继续消去元素。

   d. 如果在下方找不到非零元素，说明当前列已经全为零，需要将秩减一，并将当前列的元素全部设为最后一列对应位置的元素。

3. 最后，函数返回计算得到的秩值。

其中，行运算的具体方式为

$$ R_j=R_j-\dfrac{a_{ji}}{a_{ii}}\times R_i $$