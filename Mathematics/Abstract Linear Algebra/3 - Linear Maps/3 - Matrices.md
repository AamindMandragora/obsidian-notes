# Representing a Linear Map by a Matrix

Define an $m$-by-$n$ matrix to be a rectangular array of elements of $\mathbb F$ with $m$ rows and $n$ columns: $$A=\begin{pmatrix}A_{1,1} & \cdots & A_{1,n} \\ \vdots & \ddots & \vdots \\ A_{m,1} & \cdots & A_{m,n} \end{pmatrix}$$The matrix of a linear map from $V$ to $W$ is the $m$-by-$n$ matrix $\mathcal M(T)$ whose entries $A_{j,k}$ are defined by $Tv_k=\sum_{j=1}^mA_{j,k}w_j$, where $v_k$ and $w_j$ form bases of $V$ and $W$. We can also explicitly give the bases being sent to and from using the following notation: $$\mathcal M(T,(v_1,\ldots,v_n),(w_1,\ldots,w_m)$$Therefore, the matrix of a linear map depends on the bases chosen as well as on $T$ itself. In fact, another way to write $A$ is: $$A=\left(\begin{array}{c|c|c} Tv_1 &\cdots &Tv_n \end{array}\right)$$where $Tv_k$ is being used as a column vector.
# Addition and Scalar Multiplication of Matrices

The sum of two dimensionally-identical matrices is done by element-wise addition, and the scalar multiplication is defined by scaling each element in the matrix. This means that the $\mathcal M$ operator is a linear map, and the set of all $m$-by-$n$ matrices with entries in $\mathbb F$, $\mathbb F^{m,n}$, is a vector space with dimension $mn$.
# Matrix Multiplication

We'd ideally like $\mathcal M(ST)=\mathcal M(S)\mathcal M(T)$ for valid definitions of those linear maps, so let $A=\mathcal M(S)$ ($m\times n$) and $B=\mathcal M(T)$ ($n\times p$), then we define matrix multiplication to let $(AB)_{j,k}=\sum_{r=1}^nA_{j,k}B_{r,k}=A_{j,\cdot}B_{\cdot,k}$ for the entry in row $j$, column $k$, which makes $AB$ an $m$-by-$p$ matrix. This definition of matrix multiplication isn't commutative, but is associative and distributive.

Since a matrix product is just the matrix where each element is a row of the first multiplicand times a column of the second, the $k\text{-th}$ column of $AB$ equals $A$ times the $k\text{-th}$ column of $B$. If $A$ is an $m$-by-$n$ matrix and $b$ is an $n$-by-$1$ matrix, then $Ab=\sum_{j=1}^nb_jA_{\cdot,j}$.

Both of the results above have counterparts for the rows of a matrix. More specifically, column $k$ / row $j$ of $AB$ is a linear combination of the columns of $A$ / rows of $B$, with the coefficients coming from column $k$ of $B$ / row $j$ of $A$.
# Column-Row Factorization and Rank of a Matrix

The **column / row rank** of a matrix $A$ is the dimension of the span of the columns / rows of $A$ in $\mathbb F^{m,1}$ / $\mathbb F^{1,n}$, and it's bounded by the minimum of $m$ and $n$.

The **transpose** of a matrix $A$, denoted $A^T$ is the matrix obtained from switching rows and columns as follows: $(A^T)_{k,j}=A_{j,k}$. The transpose operator distributes over addition and both forms of multiplication.

Let $A$ have column rank $c\geq 1$, then there exists some $m$-by-$c$ and $c$-by-$n$ matrices $C$ and $R$ such that $A=CR$ by making $C$ the augmented basis of the span of the columns and $R$ the coefficients of the linear combination of those columns that equals column $k$ of $A$. This is called **column-row factorization**.

If $A\in\mathbb F^{m,n}$, then the column rank equals the row rank by the two-way bounding introduced by putting $A$ in column-row factorization. We will use the term **rank** in this case.