---
layout: default
title: "The discrete Fourier transform and orthogonality"
colour: "#8888FF"
---

$$
\newcommand{\x}{\mathbf x}
\newcommand{\y}{\mathbf y}
\newcommand{\e}{\mathbf e}
\newcommand{\f}{\mathbf f}
\newcommand{\F}{\mathbf F}
\newcommand{\bpsi}{\boldsymbol \psi}
\newcommand{\parens}[1]{\left( #1 \right)}
\newcommand{\curlies}[1]{\left\{ #1 \right\}}
\DeclareMathOperator{\proj}{proj}
$$

CS 370 at University of Waterloo is packed with content, and students come into the course with varying levels of familiarity with the theory behind the math we do. However, I think students who are familiar with linear algebra might benefit from seeing a more thorough explanation.

## The orthogonal DFT basis

The statement of orthogonality we see in class is

$$
\frac{1}{N}\displaystyle \sum_{n=0}^{N-1} W^{n(a-b)} = \delta_{a,b}
$$

In class you see a proof of this based on the geometric series formula, which I think is pretty straightforward and makes algebraic and geometric sense. But there's another way to look at it (and the reason why it's called orthogonality):

### Real orthogonality

Let's forget about Fourier transforms and think about $\R^N$ for a minute. Two vectors $\mathbf x, \mathbf y$ in $\R^N$ are orthogonal if $\x \cdot \y = 0$ ($\cdot$ denotes the dot product). Furthermore, we can make a basis for $\R^N$ by choosing $N$ linearly independent vectors, and some bases are called *orthogonal* when the dot product of any pair of basis vectors is 0. E.g. the standard basis for $\R^3$ is orthogonal:

$$
\begin{align*}
\mathcal B = \curlies{ \e_1 = (1, 0, 0), \e_2 = (0, 1, 0), \e_3 = (0, 0, 1) } \\
\e_1 \cdot \e_2 = \begin{pmatrix} 1 \\ 0 \\ 0 \end{pmatrix} \cdot \begin{pmatrix} 0 \\ 1 \\ 0 \end{pmatrix} = 1 \cdot 0 + 0 \cdot 1 + 0 \cdot 0 = 0 \\
\text{ and similar for } \mathbf e_2 \cdot \e_3 \text{ and } \e_3 \cdot \mathbf e_1
\end{align*}
$$

This basis is also *orthonormal* - any basis vector dot product itself is 1. Now we can come up with an identity for the basis vectors of $\R^3$: $\e_a \cdot \e_b$ is only $1$ if $a = b$, otherwise it is $0$, i.e. $\e_a \cdot \e_b = \delta_{a,b}$. *Does this look familiar?*

### Fourier orthogonality

Now let's go back to our Fourier domain. We can also think about the frequency domain as a vector space and do linear algebra on it. After all, $\F = (F_0, F_1, ..., F_{N-1})$ is just an $N$-dimensional complex vector. However, we use a different basis in this space:

$$
\bpsi_i = \frac{1}{\sqrt N} \parens{ W^{0i}, W^{1i}, W^{2i}, ..., W^{(N-1)i} }
$$

The standard dot product defined for $$\C^N$$ is

$$
\displaystyle \x \cdot \y = \sum_{n=0}^{N-1} x_n \overline{y_n}
$$

(Note that this isn't symmetric!)

Applying this to the basis, this means that the dot product of two basis vectors is

$$
\begin{align*}
\bpsi_a \cdot \bpsi_b &= \sum_{n=0}^{N-1} [\psi_a]_n \overline{[\psi_b]_n}\\
&= \sum_{n=0}^{N-1} \parens{ \frac{1}{\sqrt N} W^{na} } \overline{\parens{ \frac{1}{\sqrt N} W^{nb}}} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{na} \overline{W^{nb}} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{na} W^{-nb} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} W^{n(a - b)} \\
&= \delta_{a,b} \tag{as shown in class}
\end{align*}
$$

So here we recover the orthogonality identity from class, and we see that it is equivalent to saying that the DFT basis is orthonormal!

## Change of basis

With this understanding, we can now see that when we compute the discrete Fourier transform, we're actually rewriting our discrete signal in the Fourier basis, i.e. we're finding it as a linear combination of the vectors $\bpsi_0, ..., \bpsi_{N-1}$.

First let's think about the inverse Fourier transform. We know that the point of all of this is to be able to write our signal $\f$ as a sum of trigonometric functions at different frequencies, and in class we saw how that is equivalent to writing its entries as sums of powers of $W$. The corresponding formula for the IDFT tells us what we *want* to be able to do:

$$
\begin{align*}
f_n &= \sum_{k=0}^{N-1} F_k W^{nk} \\
&= \sqrt{N} \sum_{k=0}^{N-1} F_k [\bpsi_k]_n
\end{align*}
$$

Vectorizing this, we can compute the inverse Fourier transform for the whole signal at once as:

$$
\frac{1}{\sqrt{N}} \f = \sum_{k=0}^{N-1} F_k \bpsi_k
$$

This means we want to write $\f / \sqrt{N}$ as a linear combination of the $\bpsi_k$ basis vectors with coefficients $F_k$. Alternatively, we can think of it as talking about bases: we have our signal $\f$ in the standard basis, and we want to rewrite it in the DFT basis as $\F$. So how do we find the entries of $\F$?

In $\R^N$, if we wanted to find the representation of a vector in an orthonormal basis, we would project the vector onto each basis vector and get the lengths of the projections:

<img src="https://rikingurditta.github.io/blog/img/ortho-project.png" alt="image-20240719111535018" style="zoom:33%;" />

In this image, the blue vectors are the projections of the pink vector $\mathbf v$ onto each "axis", i.e. onto each orthonormal basis vector. The length of each blue vector tells us the corresponding entry when we rewrite $\mathbf v$ in our blue basis.

We can use the same approach here even though we're in $\C^N$! The projection formula we learn in first year linalg is:

$$
\proj_\y \x = \frac{\x \cdot \y}{\y \cdot \y} \y
$$

If we're projecting onto an orthonormal basis vector $\bpsi_i$, then we know $\bpsi_i \cdot \bpsi_i = 1$, so we can simplify the formula in our case to $\proj_{\bpsi_i} \x = (\x \cdot \bpsi_i) \bpsi_i$.  We also know that the length of $\bpsi_i$ is 1, so the length of the projection onto it is just $\x \cdot \bpsi_i$. That means that, using the formula for the complex dot product, our $k^\text{th}$ Fourier coefficient should be

$$
\begin{align*}
F_k &= \parens{\frac{1}{\sqrt N} \f} \cdot \bpsi_k \\
&= \frac{1}{\sqrt N} \sum_{n=0}^{N-1} f_n \overline{[\bpsi_k]_n} \\
&= \frac{1}{\sqrt N} \sum_{n=0}^{N-1} f_n \overline{\frac{1}{\sqrt N} W^{kn}} \\
&= \frac{1}{N} \sum_{n=0}^{N-1} f_n W^{-kn}
\end{align*}
$$

And so we recover the definition of the discrete Fourier transform!

### Matrix form

We've seen the matrix form of the DFT in class:

$$
M = \begin{pmatrix}
W^0 & W^0 & W^0 & \cdots & W^0 \\
W^0 & W^1 & W^2 & \cdots & W^{N-1} \\
W^0 & W^2 & W^4 & \cdots & W^{2(N-1)} \\
\vdots & \vdots & \vdots & \ddots & \vdots \\
W^0 & W^{N-1} & W^{2(N-1)} & \cdots & W^{(N-1)(N-1)} \\
\end{pmatrix}
$$

This is the change of basis matrix for exactly the task we did above!

## What's your point?

My point with all of this was to share some more abstract intuition for what we're doing, because I think otherwise the Fourier transform is pretty mysterious and magical. Many students come into this course not really knowing how complex numbers work, and without any prior experience with frequency analysis e.g. through audio (this is how I personally learned about Fourier analysis).

Linear algebra is nice because it mostly doesn't care whether your numbers are real or complex, and it helps us assign some meaning to the formulas we find in the course.

### Pitfalls

What's the *real* reason I wrote all this?

Well the reason above is truthful. But the original motivation was finding a common issue while grading CS 370 assignments. In class, we have the orthogonality identity:

$$
\sum_{n=0}^{N-1} W^{n(a-b)} = N \delta_{a,b}
$$

I've seen students use it with numbers other than $N-1$ at the top of the summation, e.g. I've seen students assert that

$$
\sum_{n=0}^{(N-1)/2} W^{n(a-b)} = \delta_{a,b}
$$

However, this isn't true! The original orthogonality identity corresponds to our derivation above, that the DFT basis is orthogonal so $\psi_a \cdot \psi_b = \delta_{a,b}$. However, if the summation isn't carried out until $N-1$ then we aren't computing the dot product, and so the identity doesn't apply here.

## Sources/Further reading

I wrote this based on the 7th chapter of the excellent book [*Fourier Analysis: An Introduction*](https://press.princeton.edu/books/hardcover/9780691113845/fourier-analysis) by Elias M. Stein and Rami Shakarchi. I highly recommend it if you liked this unit of CS 370!

I also used my own knowledge and intuition of linear algebra, and my go-to book on that is [*Linear Algebra Done Right*](https://linear.axler.net/) by Sheldon Axler (which is open access!).
