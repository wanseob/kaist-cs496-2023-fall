---
defaultTemplate:
  - ZK Lecture/templa
---
### KAIST CS496(2023 fall)<br> ZKP - Theory and Applications
Wanseob Lim<br>
PSE, EF

---

#### Lecture 7

Various Multivariate IOPs

---

Review of the Univariate Polynomial IOPs to query
1. Polynomial Identities
2. Binding Range
3. Grand Sum
4. Grand Product
5. Permutation

---

Imagine if you have 1 billion users. Interpolating 1 billion points needs $O(n \log(n))$ complexity.

---

::: title
vector $\iff$ polynomial
:::

::: left-3
$$\mathbf{v} = \begin{bmatrix} 6 \\ 2 \\ 7 \\ 9\end{bmatrix} \iff$$
:::

::: right-7
$$\begin{align} \forall (x,y) \in \{(1, 6), (2, 2), (3, 7), (4, 9)\} \\ f(x) = y \\ deg(f) \lt 4 \end{align}$$
:::

--

::: title
Univariate Polynomial
:::

::: left-7
$$f(X) = - 2 + \frac{33}{2}X - \frac{79}{2}X^{2} + 31 X^{3}$$<!-- element style="font-size:0.8em" --> 

interpolates


$$\{(1, 6), (2, 2), (3, 7), (4, 9)\}$$
:::

::: right-3
![[Pasted image 20231106223527.png]]
:::

--

::: title
Multi-linear Polynomial
:::

::: left-7
$$f(X_{1}​,X_{2}​)=6 + X_{1} − 4 X_{2}​ + 6 X_{1}X_{2}​​​$$<!-- element style="font-size:0.8em" --> 

interpolates


$$\{(0, 0, 6), (0, 1, 2), (1, 0, 7), (1, 1, 9)\}$$<!-- element style="font-size:0.8em" --> 

:::

::: right-3
![[Pasted image 20231106224445.png]]
:::

---

::: title
Transformation between vector & polynomials
:::

::: left
$$\begin{align} & \begin{bmatrix} 6 & 2 & 7 & 9\end{bmatrix}^{T} \tag{vector format}\\ & f(X) = - 2 + \frac{33}{2}X - \frac{79}{2}X^{2} + 31 X^{3} \tag{univariate poly.}\\ & f(X_{1}​,X_{2}​)=6 + X_{1} − 4 X_{2}​ + 6 X_{1}X_{2} \tag{multivariate poly.}\end{align}$$
:::<!-- element style="font-size:0.6em; margin-left: -10vw" -->

--

::: title
Vector
:::

::: left-7
$$\mathbf{v} = \begin{bmatrix} 6 \\ 2 \\ 7 \\ 9 \end{bmatrix}$$
:::
::: right-3
$$\begin{align} \\& i  & v\\ & 0 \quad \rightarrow &6 \\ & 1 \quad \rightarrow &2 \\ & 2 \quad \rightarrow &7 \\ & 3 \quad \rightarrow &9 \\ \end{align}$$
:::

--

::: title
Univariate Polynomial
:::

::: left-7
$$f(X) = - 2 + \frac{33}{2}X - \frac{79}{2}X^{2} + 31 X^{3}$$<!-- element style="font-size:0.8em" --> 
:::
::: right-3
$$\begin{align} \\& X  & Y\\ & 1 \quad \rightarrow &6 \\ & 2 \quad \rightarrow &2 \\ & 3 \quad \rightarrow &7 \\ & 4 \quad \rightarrow &9 \\ \end{align}$$
:::

--

::: title
Multivariate Polynomial
:::

::: left-7
$$f(X_{1}​,X_{2}​)=6 + X_{1} − 4 X_{2}​ + 6 X_{1}X_{2}​​​$$<!-- element style="font-size:0.8em" --> 
:::
::: right-3
$$\begin{align} \\& (X_{1}, X_{2})  & Z\\ & (0, 0) \quad \rightarrow &6 \\ & (0, 1) \quad \rightarrow &2 \\ & (1, 0) \quad \rightarrow &7 \\ & (1, 1) \quad \rightarrow &9 \\ \end{align}$$<!-- element style="font-size:0.8em" -->
:::

---

Understanding of a Multivariate Polynomial in the hyperspace

--
::: title
Understanding of a Multivariate Polynomial in the 1-dimensional hyperspace 
:::

::: left-4
$$f:\mathbb{R}\rightarrow \mathbb{R}$$
| position vector | scalar value |
| --------------- | ------------ |
| $(1)$           | $6$          |
| $(2)$           | $2$          |
| $(3)$           | $7$          |
| $(4)$           | $9$          |

$$f(X) = - 2 + \frac{33}{2}X - \frac{79}{2}X^{2} + 31 X^{3}$$
:::<!-- element style="font-size:0.6em"-->

::: right-6
![[Univariate Polynomial.png]]
:::

--
::: title
Understanding of a Multivariate Polynomial in the 2-dimensional hyperspace
:::

::: left-6
$$f:\mathbb{R}^{2} \rightarrow \mathbb{R}$$
| position vector | scalar value |
| --------------- | ------------ |
| $(1, 1)$           | $6$          |
| $(-1, 1)$           | $2$          |
| $(-1, -1)$           | $7$          |
| $(1, -1)$           | $9$          |

$$f(X_{1}, X_{2}) = \frac{3}{2} X_{1} - 2 X_{2}  + \frac{1}{2} X_{1}X_{2} + 6$$
:::<!-- element style="font-size:0.6em"-->

::: right-4
![[Bivariate Polynomial.png]]
:::


--
::: title
Understanding of a Multivariate Polynomial in the 3-dimensional hyperspace
:::

::: left-6
$$f:\mathbb{R}^{3} \rightarrow \mathbb{R}$$
| position vector | scalar value |
| --------------- | ------------ |
| $(0, 0, 0)$           | $6$          |
| $(1, 0, 0)$           | $3$          |
| $(0, 1, 0)$           | $2$          |
| $(0, 0, 1)$           | $3$          |
| $(1, 1, 0)$           | $9$          |
| $(1, 0, 1)$           | $6$          |
| $(0, 1, 1)$           | $1$          |
| $(1, 1, 1)$           | $7$          |

:::<!-- element style="font-size:0.5em"-->
::: left
$$f(X_{1}, X_{2}, X_{3}) = -3X_{1} -4X_{2} -3X_{3} + 10X_{1}X_{2} + 6X_{1}X_{3} + 2X_{2}X_{3} -7 X_{1}X_{2}X_{3} + 6$$
:::<!-- element style="font-size:0.6em; margin-top: 60vh"-->
::: right-4
![[Tri-variate Polynomial.png]]
:::

---

::: title
Multilinear Interpolation Theorem
:::

::: left
there is one and only one multilinear polynomial that interpolates a given set of $2^{n}$ points in $n$ dimensions

*** 

The most efficient structure $\implies$ interpolating $2^{n}$ vertices over an $n$-dimensional boolean hypercube
:::

---
::: title
Boolean Hypercube $B_{n}$ 
:::

::: left
$$B_{n}:=\{0, 1\}^{n}$$
*** 
*** 

$$B_{1} = \{0, 1\}$$
*** 


$$B_{2} = \{(0, 0), (0, 1), (1, 0), (1, 1)\}$$
*** 


$$\begin{align} B_{3} = \{ & (0, 0, 0), (0, 0, 1), (0, 1, 0), (1, 0, 0),\\ & (0, 1, 1), (1, 0, 1),  (1, 1, 0), (1, 1, 1)\}\end{align}$$


:::

---

Poly-IOPs using multivariate polynomials

--

Polynomial IOPs to query
1. Polynomial Identities
2. Binding Range
3. Grand Sum
4. Grand Product
5. Permutation

---

::: title
Testing polynomial identities w/ Public-coin Protocol
:::
$$F,G \in \mathbb{F}^{(\le d)}[X_{1}, X_{2}, \dots, X_{n}]$$

$$F(\mathbf{X}) \overset{?}{\equiv} G(\mathbf{X})$$
--

::: title
Testing polynomial identities w/ Public-coin Protocol
:::

::: left
$$F(\mathbf{X}) \overset{?}{\equiv} G(\mathbf{X})$$

***
#### If for some $\mathbf{r}  \overset{\$}{\leftarrow} \mathbb{F}^{n}$
$F(r)=G(r)$ and $F,G \in \mathbb{F}^{(\le d)}[X_{1}, X_{2}, \dots, X_{n}]$
#### Then
$$\mathrm{Pr}\left[\begin{align}& \forall \mathbf{X} \in \mathbb{F}^{n} \\ & F(\mathbf{X})=G(\mathbf{X}) \quad \text{i.e.} \quad F(\mathbf{X}) - G(\mathbf{X}) = 0\end{align}\right] = 1 - \frac{d}{|\mathbb{F}|}$$
:::<!-- element style="font-size:0.7em"-->

--
::: title
Testing polynomial identities w/ Public-coin Protocol
:::

::: left
$$A(\mathbf{X})B(\mathbf{X})C(\mathbf{X}) + D(\mathbf{X}) = E(\mathbf{X})F(\mathbf{X})+G(\mathbf{X})$$
:::

--
::: title
Testing polynomial identities w/ Public-coin Protocol
:::

::: left
$$A(\mathbf{X})B(\mathbf{X})C(\mathbf{X}) + D(\mathbf{X}) = E(\mathbf{X})F(\mathbf{X})+G(\mathbf{X})$$
***
#### If for some $\mathbf{r}  \overset{\$}{\leftarrow} \mathbb{F}^{n}$
$A(\mathbf{r})B(\mathbf{r})C(\mathbf{r}) + D(\mathbf{r}) = E(\mathbf{r})F(\mathbf{r}) + G(\mathbf{r})$ 

#### Then
$$\mathrm{Pr}\left[\begin{align}& \forall \mathbf{X} \in \mathbb{F}^{n} \\ & A(\mathbf{X})B(\mathbf{X})C(\mathbf{X}) + D(\mathbf{X}) = E(\mathbf{X})F(\mathbf{X})+G(\mathbf{X})\end{align}\right] = 1 - \frac{deg}{|\mathbb{F}|}$$

:::<!-- element style="font-size:0.7em"-->

--

::: title
Binding Range
:::

::: left

In univariate polynomial IOPs, we bind the range using a subgroup and its vanishing polynomial

$$F(X) = Z_{S}(X)Q(X)$$ 

$$F(X) = 0, \forall X \in S$$ 
***
In multivariate polynomial IOPs, we bind the range using the size of the hyper boolean cube

$$F(\mathbf{X}) \in \mathcal{R}, \forall \mathbf{X}\in B_{n}$$
:::

--

How to prove this?

$$F(\mathbf{X}) \in \mathcal{R}, \forall \mathbf{X}\in B_{n}$$ 

---
::: title
Starting from the sumcheck protocol
:::

::: left
There's a clever way to check the grandsum of the multivariate polynomials
:::

---

::: title
Sumcheck protocol
:::

::: left

$$S = \sum\limits_{x \in \{0, 1\}^{n}} f(x_{1}, x_{2}, \dots, x_{n})$$
***

We're going to prove that all the sum of the values of the vertices over the boolean hypercube equals to $S$

:::

--

::: title
Sumcheck protocol
:::

::: left-3
![[Tri-variate Polynomial.png]]<!-- element style="height:25vh" -->
:::

::: right-7
In this case,

$S = 6 + 3 + 2 + 3 + 9 + 6 + 1 + 7 = 37$

and we're proving that 

$$\sum\limits_{(X_{1}, X_{2}, X_{3}) \in \{0, 1\}^{3}} f(X_{1}, X_{2}, X_{3}) \equiv 37$$
:::<!-- element style="font-size:0.6em" -->

--

::: title
Sumcheck protocol - round 1:
:::

:::  left

The prover $\mathcal{P}$ computes a univariate polynomial $f_{1}(X_{1})$

$$f_{1}(X_{1}) = \sum\limits_{(x_{2}, x_{3}, \dots, x_{n}) \in \{0, 1\}^{n-1}} f(X_{1}, x_{2}, x_{3}, \dots, x_{n})$$
***

The verifier $\mathcal{V}$ accepts the claim if and only if:
1. $f_{1}(0) + f_{1}(1) = S$
2. $f_{1}$ is a polynomial of at most degree $deg_{1}(f)$

***

Subsequently, $\mathcal{V}$ sends a random $r_{1}$ to ensure that $\mathcal{P}$ is consistent with polynomial $f$.

:::<!-- element style="font-size:0.7em"-->

--

::: title
Sumcheck protocol - round 2:
:::

:::  left

The prover $\mathcal{P}$ computes a univariate polynomial $f_{2}(X_{2})$

$$f_{2}(X_{2}) = \sum\limits_{( x_{3}, \dots, x_{n}) \in \{0, 1\}^{n-2}} f(r_{1}, X_{2}, x_{3}, \dots, x_{n})$$
***


The verifier $\mathcal{V}$ accepts the claim if and only if:
1. $f_{2}(0) + f_{2}(1) = f_{1}(r_{1})$
2. $f_{2}$ is a polynomial of at most degree $deg_{2}(f)$

***

Subsequently, $\mathcal{V}$ sends a random $r_{2}$ to ensure that $\mathcal{P}$ is consistent with polynomial $f$.

:::<!-- element style="font-size:0.7em"-->

--

::: title
Sumcheck protocol - round 3:
:::

:::  left

The prover $\mathcal{P}$ computes a univariate polynomial $f_{3}(X_{3})$

$$f_{3}(X_{3}) = \sum\limits_{( x_{4}, \dots, x_{n}) \in \{0, 1\}^{n-3}} f(r_{1}, r_{2}, X_{3}, \dots, x_{n})$$
***


The verifier $\mathcal{V}$ accepts the claim if and only if:
1. $f_{3}(0) + f_{3}(1) = f_{2}(r_{2})$
2. $f_{3}$ is a polynomial of at most degree $deg_{2}(f)$

***

$\mathcal{P}$ and $\mathcal{V}$ continue this to the final round

:::<!-- element style="font-size:0.7em"-->

--

::: title
Sumcheck protocol - final round:
:::

:::  left

The prover $\mathcal{P}$ computes a univariate polynomial $f_{n}(X_{n})$

$$f_{n}(X_{n}) = \sum\limits_{x_{n} \in \{0, 1\}} f(r_{1}, r_{2}, r_{3}, \dots, X_{n})$$
***


The verifier $\mathcal{V}$ accepts the claim if and only if:
1. $f_{n}(0) + f_{n}(1) = f_{n-1}(r_{n-1})$
2. $f_{n}$ is a polynomial of at most degree $deg_{n}(f)$

***

Finally, the verifier $\mathcal{V}$ sends a final random $r_{n}$ to the prover and accepts the claim if and only if $f_{n}(r_{n}) = f(r_{1}, r_{2}, \dots, r_{n})$. We can use polynomial commitment scheme to open an evaluation of $f(r_{1}, \dots, r_{n})$.

:::<!-- element style="font-size:0.7em"-->

---

::: title
Sumcheck w/ Fiat-Shamir Transformation
:::

$$\begin{align} r_{1} & = \mathrm{H}(f_{1}) \\ & \vdots \\ r_{n} & = \mathrm{H}(f_{n})\end{align}$$

---

::: title
Multilinear Extension(MLE)
:::

::: left

For $f\in \mathbb{F}^{(\le d)}[X_{1}, \dots, X_{n}]$,

there exists a unique multilinear polynomial extension $\tilde{f}\in \mathbb{F}^{(\le 1)}[X_{1}, \dots, X_{n}]$

such that 

$\forall \mathbf{x} \in \{ 0, 1 \}^{n}, \tilde{f}(\mathbf{x}) = f(\mathbf{x})$

:::

--

::: title
Multilinear Extension(MLE)
:::

::: left

$$\tilde{f}(\mathbf{Y}) := \sum\limits_{\mathbf{X}\in \{ 0, 1 \}^{n}} f(\mathbf{X})\cdot eq(\mathbf{X}, \mathbf{Y})$$

where

$$eq(\mathbf{X}, \mathbf{Y}) = \prod \limits_{i=1}^{n}(X_{i}Y_{i} + (1 - X_{i})(1 - Y_{i}))$$

for $\mathbf{x}, \mathbf{y} \in \{ 0, 1 \}^{n}$

$$eq(\mathbf{x}, \mathbf{y}) = \begin{cases} 0 & \mathbf{x} \neq \mathbf{y}\\ 1 & \mathbf{x} = \mathbf{y} \end{cases}$$
:::<!-- element style="font-size:0.9em" -->

---

::: title
ZeroCheck $\mathcal{R}_{zero}$
:::

::: left
What we would love to prove that:

$$A(\mathbf{X})B(\mathbf{X}) = C(\mathbf{X})$$

for all $\mathbf{X} \in \{0, 1\}^{n}$
:::

--
::: title
ZeroCheck $\mathcal{R}_{zero}$
:::

::: left
What we would love to prove that:

$$F(\mathbf{X}) = A(\mathbf{X})B(\mathbf{X}) - C(\mathbf{X})$$

for all $\mathbf{X} \in \{0, 1\}^{n}$
:::

--

::: title
ZeroCheck $\mathcal{R}_{zero}$
:::

::: left

$\hat{F}(\mathbf{X}) = F(\mathbf{X})\cdot eq(\mathbf{X}, \mathbf{r})$

for a random $\mathbf{r} \overset{\$}\leftarrow \mathbb{F}^{n}$

If $\sum\limits_{\mathbf{x}\in\{ 0, 1 \}^{n}} \hat{F}(\mathbf{x}) = 0$

$F(\mathbf{x})=0, \forall \mathbf{x}\in \{ 0, 1 \}^{n}$

:::

--

::: title
ZeroCheck $\mathcal{R}_{zero}$
:::

::: left
$$F(\mathbf{X}) = A(\mathbf{X})B(\mathbf{X}) - C(\mathbf{X})$$
for a random $\mathbf{r, r'} \overset{\$}\leftarrow \mathbb{F}^{n}$

1. $\sum \hat{F} = 0$
1. $\left(A(\mathbf{r})B(\mathbf{r}) - C(\mathbf{r})\right)\cdot eq(\mathbf{r}, \mathbf{r'}) = \hat{F}(\mathbf{r})$

:::

--

::: title
ZeroCheck $\mathcal{R}_{zero}$
:::

::: left

$\sum\limits_{\mathbf{x}\in \{ 0, 1 \}^{n}} \hat{F}(\mathbf{x}) = \sum\limits_{\mathbf{x}\in \{ 0, 1 \}^{n}}  F(\mathbf{x})\cdot eq(\mathbf{x}, \mathbf{r}) = \tilde{F}(\mathbf{r})$

for a random $\mathbf{r} \overset{\$}\leftarrow \mathbb{F}^{n}$

If $\sum\limits_{\mathbf{x}\in\{ 0, 1 \}^{n}} \hat{F}(\mathbf{x}) = \tilde{F}(\mathbf{r}) =  0$

$\tilde{F}(\mathbf{X})=0 \iff F(\mathbf{x})=0, \forall \mathbf{x}\in \{ 0, 1 \}^{n}$

:::

---

::: title
Multivariate Polynomial Commitment Schemes
:::

::: left
if $F(\mathbf{a}) = v$
$$F(\mathbf{X}) - v = \sum (X_{i} - a_{i})Q_{i}(\mathbf{X})$$
:::

---

::: title
ProdCheck $\mathcal{R}_{prod}$
:::

::: left
We would love to check that we have two different multivariate polynomials $F,G \in \mathbb{F}^{(\le 1)}[X_{1}, \dots, X_{n}]$ and

$$\prod\limits_{\mathbf{X}\in\{ 0, 1 \}^{n}} F(\mathbf{X}) \overset{?}{=} \prod\limits_{\mathbf{X}\in\{ 0, 1 \}^{n}} G(\mathbf{X})$$
:::

--

::: title
ProdCheck $\mathcal{R}_{prod}$
:::

::: left-5
We'll define an auxiliary Multilinear Polynomial $\tilde{v}\in \mathbb{F}^{(\le 1)}[X_{1}, \dots, X_{n+1}]$

such that

$$\tilde{v}(0, X_{1}, \dots, X_{n}) \equiv \tilde{f}(X_{1}, \dots, X_{n})$$

and

$$\tilde{v}(1, X_{1}, \dots, X_{n}) \equiv \tilde{v}(X_{1}, \dots, X_{n}, 0)\cdot \tilde{v}(X_{1}, \dots, X_{n}, 1)$$

where $\tilde{f}$ is a multilinear extension such that   

$\tilde{f}(\mathbf{x}) = \frac{F(\mathbf{x})}{G(\mathbf{x})}, \forall \mathbf{x} \in \{ 0, 1 \}^{n}$

:::<!-- element style="font-size:0.7em" -->

--

::: title
ProdCheck $\mathcal{R}_{prod}$
:::

::: left
$\hat{r}_{1}(\mathbf{X}) = \tilde{v}(1, \mathbf{X}) - \tilde{v}(\mathbf{X}, 0)\cdot \tilde{v}(\mathbf{X}, 1)$ and $\hat{r}_{1}(\mathbf{x}) \equiv 0 \quad \forall \mathbf{x} \in \{ 0, 1 \}^{n}$


$\hat{r}_{2}(\mathbf{X}) = \tilde{v}(0, \mathbf{X})G(\mathbf{X}) - F(\mathbf{X})$ and $\hat{r}_{2}(\mathbf{x}) \equiv 0 \quad \forall \mathbf{x} \in \{ 0, 1 \}^{n}$

***

$$\hat{h}(X_{0}, X_{1}, \dots, X_{n}) = (1 - X_{0})\cdot \hat{r}_{1}(\mathbf{X}) + X_{0}\cdot \hat{r}_{2}(\mathbf{X})$$

and

$$\hat{h}(\mathbf{x}) \equiv 0, \forall \mathbf{x}\in\{ 0, 1 \}^{n+1}$$
:::<!-- element style="font-size:0.7em" -->

---

::: title
Permutation $\mathcal{R}_{perm}$
:::

::: left
$$(F + \gamma, G + \gamma) \in \mathcal{R}_{prod}$$
:::

---

::: title
$\sigma$-Permutation $\mathcal{R}_{\sigma-perm}$
:::

::: left
$$\sigma: \{ 0, 1 \}^{n} \rightarrow \{ 0, 1 \}^{n}$$

And we would love to check that for two different multivariate polynomials $F,G \in \mathbb{F}[X_{1}, \dots, X_{n}]$


$$F(\sigma(\mathbf{x})) \equiv G(\mathbf{x}), \forall \mathbf{x} \in \{ 0, 1 \}^{n}$$
:::

--

::: title
$\sigma$-Permutation $\mathcal{R}_{\sigma-perm}$
:::

::: left

$s_{bits2Num}$: 
Transforms the boolean hypercube vector into a scalar value

$s_{\sigma}$:
Transforms the $\sigma$ permutation of a boolean hypercube vector into a scalar value
:::

--

::: title
$\sigma$-Permutation $\mathcal{R}_{\sigma-perm}$
:::

::: left

$$(F + \gamma + \delta s_{bits2Num}, G + \gamma + \delta s_{\sigma}) \in \mathcal{R}_{prod}$$
:::

---

QnA