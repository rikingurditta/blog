---
layout: default
title: "The discrete Fourier transform and orthogonality"
colour: "#BBBBFF"
---

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\b}{\mathbf b}
\newcommand{\e}{\mathbf e}
\newcommand{\f}{\mathbf f}
\newcommand{\p}{\mathbf p}
\newcommand{\v}{\mathbf v}
\newcommand{\F}{\mathbf F}
\newcommand{\parens}[1]{\left( #1 \right)}
\newcommand{\curlies}[1]{\left\{ #1 \right\}}
\DeclareMathOperator{\proj}{proj}
$$

*CS 370 - Numerical Computation* at University of Waterloo is packed with content, and students come into the course with varying levels of familiarity with the theory behind the math we do. However, I think students who are familiar with linear algebra might benefit from seeing a more thorough explanation.

If you haven't taken CS 370 or aren't currently taking it, you probably won't have enough context for this post.

## The orthogonal DFT basis

The statement of orthogonality we see in class is

$$
\frac{1}{N}\displaystyle \sum_{n=0}^{N-1} W^{n(a-b)} = \delta_{a,b}
$$

In class you see a proof of this based on the geometric series formula, which I think is pretty straightforward and makes algebraic and geometric sense. But there's another way to look at it (and the reason why it's called orthogonality):

### Real orthogonality

Let's forget about Fourier transforms and think about $\R^N$ for a minute. Two vectors $\mathbf x, \mathbf y$ in $\R^N$ are orthogonal if $\x \cdot \y = 0$ ($\cdot$ denotes the dot product). Furthermore, we can make a basis for $\R^N$ by choosing $N$ linearly independent vectors, and we can make it *orthogonal* by making sure the dot product of any pair of basis vectors is 0. For example, the standard basis for $\R^3$ is orthogonal:

$$
\begin{align*}
\mathcal B = \curlies{ \e_0 = (1, 0, 0), \e_1 = (0, 1, 0), \e_2 = (0, 0, 1) } \\
\e_0 \cdot \e_1 = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} = 1 \cdot 0 + 0 \cdot 1 + 0 \cdot 0 = 0 \\
\text{ and similar for } \mathbf e_1 \cdot \e_2 \text{ and } \e_2 \cdot \mathbf e_0
\end{align*}
$$

This basis is also *orthonormal* - any basis vector dot product itself is 1. Now we can come up with an identity for the basis vectors of $\R^3$: $\e_a \cdot \e_b = 1$ if $a = b$, otherwise it is $0$. More concisely, $\e_a \cdot \e_b = \delta_{a,b}$. Expanding this out, we get

$$
\sum_{n=0}^2 [\e_a]_n [\e_b]_n = \delta_{a, b}
$$

*Does this look familiar?*

### Fourier orthogonality

Now let's go back to our Fourier domain. We can also think about the frequency domain as a vector space and do linear algebra on it. After all, $\F = (F_0, F_1, ..., F_{N-1})$ is just an $N$-dimensional complex vector. However, we use a different basis in this space:

$$
\b_i = \frac{1}{\sqrt N} \parens{ W^0, W^i, W^{2i}, ..., W^{(N-1)i} }
$$

(so the $n^\text{th}$ entry of the $i^\text{th}$ basis vector is $\displaystyle [\b_i]_n = \frac{1}{\sqrt N} W^{ni}$)

The standard dot product defined for $$\C^N$$ is

$$
\displaystyle \x \cdot \y = \sum_{n=0}^{N-1} x_n \overline{y_n}
$$

where $\overline{a + bi} = a - bi$ is the complex conjugate. (Note that this isn't symmetric!)

Applying this to the basis, we can calculate the dot product between any two basis vectors:

$$
\begin{align*}
\b_a \cdot \b_b &= \sum_{n=0}^{N-1} [\b_a]_n \overline{[\b_b]_n}\\
&= \sum_{n=0}^{N-1} \parens{ \frac{1}{\sqrt N} W^{na} } \overline{\parens{ \frac{1}{\sqrt N} W^{nb}}} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{na} \overline{W^{nb}} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{na} W^{-nb} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{n(a - b)} \\
&= \delta_{a,b} \tag{as shown in class}
\end{align*}
$$

So here we recover the orthogonality identity from class, and we see that it is equivalent to saying that the DFT basis is orthonormal!

## Change of basis

We can go further with this linear algebra view of discrete Fourier analysis. First let's think about the inverse Fourier transform. We know that the point of all of this is to be able to write our signal $\f = (f_0, ..., f_{N-1})$ as a sum of trigonometric functions with different frequencies. We class we saw how this is equivalent to writing its entries as sums of powers of $W$. This goal for our coefficients is expressed in the formula for the inverse DFT:

$$
\begin{align*}
f_n &= \sum_{k=0}^{N-1} F_k W^{nk} \\
&= \sum_{k=0}^{N-1} F_k (\sqrt N [\b_k]_n)
\end{align*}
$$

Vectorizing this, we can compute the inverse Fourier transform for the whole signal at once as:

$$
\frac{1}{\sqrt N} \f = \sum_{k=0}^{N-1} F_k \b_k
$$

This means we want to write $\f / \sqrt N$ as a linear combination of the $\b_k$ basis vectors with coefficients $F_k$. Alternatively, we can think of it as talking about bases: We have our signal $\f$ in the standard basis (our original domain), and we want to rewrite it in the DFT basis as $\F$. So how do we find the entries of $\F$?

### Matrix form

We know from linear algebra that we can construct a matrix to go between two bases. If we have a vector $\v$ in a basis $\mathcal B$, we can find its representation in the standard basis by multiplying $\v$ by the matrix whose columns are the basis vectors in $\mathcal B$:

$$
\text{if our basis is } \mathcal B = \curlies{\p_i}_{i=0}^{N-1} \\
\text{ then the change of basis matrix is } M_{\mathcal B \to std} = \begin{pmatrix} \vdots & & \vdots \\ \p_0 & \cdots & \p_{N-1} \\ \vdots & & \vdots\end{pmatrix}
$$

So the matrix to go from our DFT basis to the standard basis would be

$$
M_{DFT \rightarrow std} = \begin{pmatrix} \vdots & & \vdots \\ \b_0 & \cdots & \b_{N-1} \\ \vdots & & \vdots \end{pmatrix} =
\frac{1}{\sqrt N} \begin{pmatrix}
W^0 & W^0 & W^0 & \cdots & W^0 \\
W^0 & W^1 & W^2 & \cdots & W^{N-1} \\
W^0 & W^2 & W^4 & \cdots & W^{2(N-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
W^0 & W^{N-1} & W^{2(N-1)} & \cdots & W^{(N-1)(N-1)}
\end{pmatrix}
$$

So the inverse of this matrix should take us from the standard basis (original domain) to the DFT basis. From linear algebra, we know that if the columns of a matrix are orthonormal, then its inverse is its conjugate transpose (its *adjoint*), so

$$
M_{std \rightarrow DFT} = \frac{1}{\sqrt N} \begin{pmatrix}
W^0 & W^0 & W^0 & \cdots & W^0 \\
W^0 & W^{-1} & W^2 & \cdots & W^{-(N-1)} \\
W^0 & W^{-2} & W^4 & \cdots & W^{-2(N-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
W^0 & W^{-(N-1)} & W^{-2(N-1)} & \cdots & W^{-(N-1)(N-1)}
\end{pmatrix}
$$

So if we want to write $\f/\sqrt N$ in the DFT basis, we multiply it by the change of basis above, and we recover the DFT matrix we saw in class!

$$
\F = M_{std \rightarrow DFT} \parens{\frac{1}{\sqrt N} \f} = \underbrace{\frac{1}{N} \begin{pmatrix}
W^0 & W^0 & W^0 & \cdots & W^0 \\
W^0 & W^{-1} & W^2 & \cdots & W^{-(N-1)} \\
W^0 & W^{-2} & W^4 & \cdots & W^{-2(N-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
W^0 & W^{-(N-1)} & W^{-2(N-1)} & \cdots & W^{-(N-1)(N-1)}
\end{pmatrix}}_M \f
$$

### Element-wise form

Consider formula above, but specifically for the $k^\text{th}$ element of $\F$. The way matrix multiplication works, we compute this by taking the dot product between $\f$ and the $k^\text{th}$ row of $M$:

$$
\begin{align*}
\begin{pmatrix} \\ \\ F_k \\ \\ \\ \end{pmatrix} &= \frac{1}{N} \begin{pmatrix} \\ \\ W^0 & W^{-k} & W^{-2k} & \cdots & W^{-(N-1)k} \\ \\ \\ \end{pmatrix} \begin{pmatrix} f_0 \\ \\ \vdots \\ \\ f_{N-1} \end{pmatrix} \\
F_k &= \frac{1}{N} \sum_{n=0}^{N-1} f_n W^{-nk}
\end{align*}
$$

And so we recover the element-wise definition of the DFT from class!

## What's your point?

My point with all of this was to share some more abstract theory for what we're doing and hopefully impart some linear algebra intuition for it. I think without that, the Fourier transform is pretty mysterious and magical. I probably haven't cleared up the *meaning* of what it's doing (This thing takes a function of $t$ and returns a function of $\omega$? What does that even mean????) but I'm happy if I've clarified why it has some of its properties. I also hope this understanding of our manipulations of DFTs helps students avoid common errors.

### Pitfalls

This post is a heavily edited and expanded version of a Piazza post I made in Winter 2024, after discovering a common misunderstanding while grading CS 370 assignments. In class, we have the orthogonality identity:

$$
\sum_{n=0}^{N-1} W^{n(a-b)} = N \delta_{a,b}
$$

We know from the first section of this article that this is really a restatement of the orthonormality of the DFT basis:

$$
\sum_{n=0}^{N-1} W^{n(a-b)} = N \b_a \cdot \b_b = N\delta_{a, b}
$$

One mistake that can come from not understanding the linear algebra point of view is trying to use the formula when the summation doesn't go up to $N-1$. For example, I saw students assert that

$$
\sum_{n=0}^{(N-1)/2} W^{n(a-b)} = N \delta_{a,b}
$$

This isn't true! Since this summation doesn't go all the way up to $N$, it *doesn't* correspond to the dot product between $\b_a$ and $\b_b$, so we can't just assert that the same identity holds.

## Sources/Further reading

I wrote this based on the 7th chapter, "*Finite Fourier Analysis*" of the excellent book [*Fourier Analysis: An Introduction*](https://press.princeton.edu/books/hardcover/9780691113845/fourier-analysis) by Elias M. Stein and Rami Shakarchi. I highly recommend it if you liked this unit of CS 370!

I also used my own knowledge and intuition of linear algebra, and my go-to book on that is [*Linear Algebra Done Right*](https://linear.axler.net/) by Sheldon Axler (which is open access!). Chapter 7 on "*Operators on Inner Product Spaces*" (aka *relationships between linear transformations and dot products*) is particularly relevant here.
