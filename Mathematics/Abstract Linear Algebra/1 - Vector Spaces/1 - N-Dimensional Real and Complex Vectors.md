# Complex Numbers

We're already very familiar with the basic properties of the set of real numbers $\mathbb R$. One such property is that there's no defined square root for a negative real number, which is why we created the set of complex numbers $\mathbb C$, assuming that we have some $i=\sqrt 1$ that obeys the usual laws of arithmetic.

A **complex number** is an ordered pair $(a, b)\in \mathbb C$ where $a,b\in\mathbb R$, but we can also write it in the form $a+bi$. Therefore, we can write the set of complex numbers as $$\mathbb C=\set{a+bi:a,b\in\mathbb R}$$We define addition and multiplication on $\mathbb C$ by: $$\begin{align}(a+bi)+(c+di)=(a+c)+(b+d)i \\ (a+bi)(c+di)=(ac-bd)+(ad+bc)i\end{align}$$where $a,b,c,d\in\mathbb{R}$. If $a\in\mathbb{R}$, then we can write $a=a+0i\in\mathbb{C}$, making the real numbers a subset of the complex numbers. 

The definition of complex addition and multiplication follows from applying the rules of real algebra and assuming $\sqrt{-1}=i$. Therefore, both are commutative, associative, have identities and inverses, and satisfy the distributive property. The proofs of those properties also use real algebra rules and the definitions above.

We will define the additive and multiplicative inverses of a complex number $\alpha$ to be $-\alpha$ and $1/\alpha$, and we define subtraction and division to be addition and multiplication of the operand's inverse. Since we can define all four operations like this, $\mathbb{C}$ is a field, just like $\mathbb{R}$. We will use the symbol $\mathbb{F}$ to denote a more general field, such that any theorems proved on $\mathbb{F}$ can be extended to the real and complex numbers.

Elements of $\mathbb{F}$ are called **scalars**, as opposed to vectors, which will be defined later. We define $\alpha^m$ to be the product of some scalar $\alpha$ by itself $m$ times, implying that $(\alpha^m)^n=\alpha^{mn}$ and $(\alpha\beta)^m=\alpha^m\beta^m$ for all scalar $\alpha,\beta$ and positive integer $m, n$.
# Lists

The sets $\mathbb{R}^2$ and $\mathbb{R}^3$ contain all ordered pairs and triples of real numbers. To generalize them to higher dimensions, we must first discuss the concept of lists. A **list** of length $n$, or $n\text{-tuple}$, is an ordered collection of $n$ elements, where $n$ is a nonnegative integer. These elements can be anything, not just numbers. Two lists are equal if and only if they have the same length and the same elements in the same order. A list of length $0$ looks like $()$, and is considered a list only so theorems don't have trivial exceptions.
# N-Dimensional Scalars

For the rest of this section, every $n$ will be referencing the same arbitrary positive integer.

$\mathbb{F}^n$ is the set of all scalar lists of length $n$: $$\mathbb{F}^n=\set{(x_1,\ldots,x_n):x_k\in\mathbb{F}\text{ for }k=1,\ldots,n}$$We say that $x_k$ is the $k\text{-th}$ coordinate of the list.

We live in a three dimensional world, so we can't visualize $\mathbb{R}^n$ as a physical object if $n>3$. Similarly, $\mathbb{C}$ can be thought of as a plane where the coordinates are the real and imaginary coefficients, but we can't create an image any higher degree of the list. Despite this, we can easily perform algebra in $\mathbb{F}^n$.

Addition in $\mathbb{F}^n$ is defined as follows: $$(x_1,\ldots,x_n)+(y_1,\ldots,y_n)=(x_1+y_1,\ldots,x_n+y_n)$$and therefore follows the rules for real addition (associative and commutative).

Let $0$ denote the list of length $n$ whose coordinates are all $0$, which won't ever be ambiguous as addition between a list and a scalar isn't defined, as is multiplication between two lists. Now, $0$ is an additive identity, and we can derive unique inverses of lists from that.
# Digression on Fields

A field is a set containing two operations (we will default to addition and multiplication), where each one has a distinct identity element and inverse function, that satisfy commutativity, associativity, and the distributive property. Therefore, $\mathbb{R}$ and $\mathbb{C}$ are both fields, as are $\mathbb{Q}$ and $\mathbb{Z}_2$. We won't deal with any fields other than $\mathbb{R}$ and $\mathbb{C}$ here, but many of the properties we'll use will also work for arbitrary fields, which we'll denote as $\mathbb{F}$. At least, they'll work for arbitrary algebraically closed fields where $1+1\neq 0$.