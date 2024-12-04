---
defaultTemplate:
  - "[[cs496-2023-fall]]"
---
[]()### KAIST CS496(2023 fall)<br> ZKP - Theory and Applications
Wanseob Lim<br>
PSE, EF

---

#### Lecture 8

Lookup arguments & Custom Gates

---

::: title
What does the lookup argument do?
:::
::: left
1. Let's say there exists a function that takes $n$ field elements as inputs and returns a field element
$f: \mathbb{F}^{n} \rightarrow \mathbb{F}$
2. And imagine that evaluating $f$ with inputs are not zk-friendly(precisely, non-friendly for arithemtic circuits)
3. We can precompute all the cases and lookup the result
:::

--

::: title
Example (XOR)
:::
::: left
| input 1   | input 2   | output   |
| --- | --- | --- |
| 00 (0) |  00  (0) |  00  (0)|
| 00 (0) |  01  (1) | 01  (1)|
| 00 (0) |  10  (2) | 10  (2)|
| 00 (0) |  11  (3) |  11  (3)|
| 01 (1) |  00  (0) |  01  (1)|
| 01 (1) |  01  (1) |  00  (0)|
| 01 (1)  |  10 (2)  |  11  (3)|
| 01 (1)  |  11 (3)  |  10  (2)|
| 10 (2)  |  00  (0) |  10  (2)|
| 10 (2)  |  01 (1)  |  11 (3) |
| 10 (2)  |  10 (2)  |  00 (0) |
| 10 (2)  |  11 (3)  |  01 (1) |
| 11 (3)  |  00 (0)  |  11 (3) |
| 11 (3)  |  01 (1)  |  10 (2) |
| 11 (3)  |  10 (2)  |  01 (1) |
| 11 (3)  |  11 (3)  |  00 (0) | 
:::<!-- element style="font-size:0.35em" -->

--

::: title
How to?
:::
::: left-5
### Table 

| input 1   $\times \alpha$ |  input 2  $\times \beta$  | output   $\times \gamma$ | table $t_{i}$|
| --- | --- | --- | --- |
| 00 (0) |  00  (0) |  00  (0)|  0 |
| 00 (0) |  01  (1) | 01  (1)| $\beta + \gamma$| 
| 00 (0) |  10  (2) | 10  (2)| $2\beta + 2 \gamma$ |
| 00 (0) |  11  (3) |  11  (3)| $3\beta + 3\gamma$ |
| 01 (1) |  00  (0) |  01  (1)| $\alpha + \gamma$ |
| 01 (1) |  01  (1) |  00  (0)| $\alpha + \beta$ |
| 01 (1)  |  10 (2)  |  11  (3)| $\alpha + 2 \beta + 3 \gamma$ |
| 01 (1)  |  11 (3)  |  10  (2)| $\alpha + 3 \beta + 2 \gamma$ |
| 10 (2)  |  00  (0) |  10  (2)| $2 \alpha + 2 \gamma$ |
| 10 (2)  |  01 (1)  |  11 (3) | $2 \alpha + \beta + 3 \gamma$ |
| 10 (2)  |  10 (2)  |  00 (0) | $2 \alpha + 2 \beta$ |
| 10 (2)  |  11 (3)  |  01 (1) | $2 \alpha + 3 \beta + \gamma$ |
| 11 (3)  |  00 (0)  |  11 (3) | $3\alpha + 3 \gamma$ |
| 11 (3)  |  01 (1)  |  10 (2) | $3 \alpha + \beta + 2 \gamma$ |
| 11 (3)  |  10 (2)  |  01 (1) | $3 \alpha + 2 \beta + \gamma$ |
| 11 (3)  |  11 (3)  |  00 (0) |  $3 \alpha + 3 \beta$ |
:::<!-- element style= "font-size:0.35em" -->

::: right-5

### Witness 

| $X$ | $L(X)$   $\times \alpha$ |  $R(X)$  $\times \beta$  | $O(X)$   $\times \gamma$ | Witness $W(X)$|
| --- | --- |--- | --- | --- |
| $X_{1}$ | 10 (2) |  01 (1) |  11  (3)|  $2\alpha + \beta + 3 \gamma$ |
| $X_{2}$ | 01 (1) |  10 (2) | 11  (3)| $\alpha + 2 \beta + 3 \gamma$| 
| $X_{3}$ | 11 (3) |  11 (3) | 00  (0)| $3 \alpha + 3 \beta$ |
| $X_{4}$ | 00 (0) |  11 (3) |  11  (3)| $3 \beta + 3 \gamma$ |
| $X_{5}$ | 01 (1) |  01 (1) |  00  (0)| $\alpha + \beta$ |
| $X_{6}$ | 11 (3) |  11 (3) |  00  (0)| $3 \alpha + 3 \beta$ |
| $X_{7}$ | 11 (3) |  00 (0)  |  11  (3)| $3\alpha + 3 \gamma$ |

Question:

Is all $\{ W(X_{i}) \}_{i \in [n=7]} \subset \{ T_{j} \}_{j\in[d=16]}$
:::<!-- element style= "font-size:0.35em; padding-bottom:15vh" -->

---

::: title
Naive Univariate Approach
:::
::: left
1. Let's say $F(X)$ interpolates your witness $\{ f_{1}, \dots, f_{n} \}$ over $\mathbb{H}_{n}$

1. And you have a prefixed table polynomial $T(X)$ that interpolates the table $\{ t_{1}, \dots, t_{d} \}$ with size $d$ over $\mathbb{H}_{d}$
***
The Naive design:

1. $F(X)-T(\sigma(X))$ returns 0 over $\mathbb{H}_{n}$
2. $\sigma: \mathbb{H}_{n} \rightarrow \mathbb{H}_{d}$ is a polynomial that interpolates $\{ (\omega_{n}^{i}, \omega_{d}^{j})\}_{i\in[n]}$  such that $F(\omega_{n}^{i}) = T(\omega_{d}^{j})$
***
PIOP constraints:
1. $F(X) - T(\sigma(X)) = Z_{n}(X)Q(X)$
2. $(\sigma(X))^{d} - 1 = Z_{n}(X)Q(X)$
***
Complexity:
- $O(nd \log(nd))$ : more than quadratic
:::<!-- element style="font-size:0.5em" -->

--

::: title
For Multivariate Polynomial
:::
::: left
For multivariate polynomial, permutation based lookup argument works in a quasi-linear way
:::

--
::: title
Linear Transformation based Multivariate Lookup
:::
::: left
Let's define a transformation function

$\sigma: \{ 0, 1 \}^{n} \rightarrow \{ 0, 1 \}^{d}$  

In this case $\sigma$ can be defined as a set of polynomials to express the linear transformation

$$
\sigma :=\{  \sigma_{1}, \dots, \sigma_{d}\}(X)
$$

Then, for all over the boolean hypercube of $\{ 0, 1 \}^{n}$ for the witness domain, 

$$\begin{align}& F(\mathbf{X}) - T(\sigma(\mathbf{X})) = 0 \\\iff & F(\mathbf{X}) - T(\sigma_{1}(\mathbf{X}), \dots, \sigma_{d}(\mathbf{X})) = 0\end{align}$$

along with it proving 1 more relation that the $\sigma_{i}$ will return only 0, 1

$$\begin{align}& \sigma_{i}(\mathbf{X})\cdot (\sigma_{i}(\mathbf{X}) - 1) = 0 \\& \forall \mathbf{X} \in \{ 0, 1 \}^{n}\end{align}$$
:::<!-- element style="font-size:0.6em" -->

---

::: title
Plookup - introduces quasi-linear
:::
::: left

Let's say we have a table $t=\{ t_{i} \}_{i\in[d]}$
and witness $f=\{ f_{i} \}_{i\in[n]}$

$t = \{ t_{1}, t_{2}, t_{3}, t_{4}, t_{5}, t_{6}, t_{7}  \}$ when $d$ = 7
and for example when $n=6$
$f = \{ \underset{t_{1}}{f_{1}}, \underset{t_{1}}{f_{2}},\underset{t_{1}}{f_{3}}, \underset{t_{2}}{f_{4}}, \underset{t_{2}}{f_{5}},\underset{t_{3}}{f_{6}}\}$ 

In this case we say
- [ ] $f \subset t$ 
- $f$  is sorted by $t$

Then, we'll merge two tables $t, w$ and make a merged set $s$ 
and $s = \mathrm{sort}_{t}(t \| f) = \{\overbrace{\underset{s_{1}}{f_{1}}, \underset{s_{2}}{f_{2}}, \underset{s_{3}}{f_{3}}, \underset{s_{4}}{t_{1}}}^{t_{1}}, \overbrace{\underset{s_{5}}{f_{4}}, \underset{s_{6}}{f_{5}}, \underset{s_{7}}{t_{2}}}^{t_{2}}, \overbrace{\underset{s_{8}}{f_{6}}, \underset{s_{9}}{t_{3}}}^{t_{3}}, \underset{s_{10}}{t_{4}}, \underset{s_{11}}{t_{5}}, \underset{s_{12}}{t_{6}}, \underset{s_{13}}{t_{7}}\}$
:::<!-- element style="font-size:0.7em"-->

--
::: title
Plookup - introduces quasi-linear
:::
::: left

Main assumption,

If $s$ is sorted correctly,
- if $s_{i+1}=s_{i}$, 
	- $(s_{i} + \beta\cdot s_{i+1}) = (1+\beta) s_{i}$
- else
	- $(s_{i} + \beta\cdot s_{i+1}) = (t_{j}+\beta\cdot t_{j+1})$ for some $j$

You can observe that

:::<!-- element style="font-size:0.7em"-->

--

::: left-5
$$\begin{align}& f_{i}' = (f_{i} + \gamma)\cdot \gamma^{-1}  = 1 + \gamma^{-1} f_{i}\\ \\\hline& f_{1}' = 1 + \gamma^{-1}\cdot f_{1} = 1 + \gamma^{-1}\cdot t_{1} \\& f_{2}' = 1 + \gamma^{-1}\cdot f_{2} = 1 + \gamma^{-1}\cdot t_{1} \\& f_{3}' = 1 + \gamma^{-1}\cdot f_{3} = 1 + \gamma^{-1}\cdot t_{1} \\& f_{4}' = 1 + \gamma^{-1}\cdot f_{4} = 1 + \gamma^{-1}\cdot t_{2} \\& f_{5}' = 1 + \gamma^{-1}\cdot f_{5} = 1 + \gamma^{-1}\cdot t_{2} \\& f_{6}' = 1 + \gamma^{-1}\cdot f_{6} = 1 + \gamma^{-1}\cdot t_{3} \\\hline \\ \\\prod_{i\in[n=6]} & f_{i}' = (1 + \gamma^{-1}\cdot t_{1})^{3}\cdot(1 + \gamma^{-1}\cdot t_{2})^{2}\cdot(1 + \gamma^{-1}\cdot t_{3})^{1}\end{align}$$
:::<!-- element style="font-size:0.4em" -->
::: right-5
$$\begin{align}& t_{i}' = \frac{(1 + \gamma^{-1})\cdot (t_{i} + \beta \cdot t_{i+1})}{1 + \beta} \\ \\\hline& t_{1}' = \frac{(1 + \gamma^{-1})\cdot (t_{1} + \beta \cdot t_{2})}{1 + \beta} \\ \\& t_{2}' = \frac{(1 + \gamma^{-1})\cdot (t_{2} + \beta \cdot t_{3})}{1 + \beta} \\ \\& t_{3}' = \frac{(1 + \gamma^{-1})\cdot (t_{3} + \beta \cdot t_{4})}{1 + \beta} \\ \\& t_{4}' = \frac{(1 + \gamma^{-1})\cdot (t_{4} + \beta \cdot t_{5})}{1 + \beta} \\ \\& t_{5}' = \frac{(1 + \gamma^{-1})\cdot (t_{5} + \beta \cdot t_{6})}{1 + \beta} \\ \\& t_{6}' = \frac{(1 + \gamma^{-1})\cdot (t_{6} + \beta \cdot t_{7})}{1 + \beta} \\ \\\hline \\ \\\prod_{i\in[d-1]} & t_{i}' = \frac{(1 + \gamma^{-1})\cdot (t_{1} + \beta \cdot t_{2})}{1 + \beta} \cdots \frac{(1 + \gamma^{-1})\cdot (t_{6} + \beta \cdot t_{7})}{1 + \beta}\end{align}$$
:::<!-- element style="font-size:0.4em" -->

--

::: left-5
$$\begin{align}& s_{i}' = s_{i} + \beta \cdot s_{i+1} \\ \\\hline& s_{1}' = s_{1} + \beta \cdot s_{2} = (1+\beta)t_{1} \\& s_{2}' = s_{2} + \beta \cdot s_{3} = (1+\beta)t_{1}\\& s_{3}' = s_{3} + \beta \cdot s_{4} = (1+\beta)t_{1} \\& s_{4}' = s_{4} + \beta \cdot s_{5} = (t_{1} + \beta \cdot t_{2})\\& s_{5}' = s_{5} + \beta \cdot s_{6} = (1+\beta)t_{2} \\& s_{6}' = s_{6} + \beta \cdot s_{7} = (1+\beta)t_{2} \\& s_{7}' = s_{7} + \beta \cdot s_{8} = (t_{2} + \beta \cdot t_{3})\\& s_{8}' = s_{8} + \beta \cdot s_{9} = (t_{2} + \beta \cdot t_{3})\\ \\& s_{9}' = s_{9} + \beta \cdot s_{10} = (t_{3} + \beta \cdot t_{4})\\ \\& s_{10}' = s_{10} + \beta \cdot s_{11} = (t_{4} + \beta \cdot t_{5})\\ \\& s_{11}' = s_{11} + \beta \cdot s_{12} = (t_{5} + \beta \cdot t_{6})\\ \\& s_{12}' = s_{12} + \beta \cdot s_{13} = (t_{6} + \beta \cdot t_{7})\\ \\\hline \\ \\\prod_{i\in[n+d-1]} & s_{i}' = ((1+\beta)t_{1})^{3}\cdot((1+\beta)t_{2})^{2}\cdot((1+\beta)t_{3})^{1} \\& \quad\quad \cdot(t_{1}+\beta t_{2}) \\& \quad\quad \cdot(t_{2}+\beta t_{3}) \\& \quad\quad \cdot(t_{3}+\beta t_{4}) \\& \quad\quad \cdot(t_{4}+\beta t_{5}) \\& \quad\quad \cdot(t_{5}+\beta t_{6}) \\& \quad\quad \cdot(t_{6}+\beta t_{7}) \\\end{align}$$

:::<!-- element style="font-size:0.3em" -->
::: right-5
$$\begin{align}& s_{i}'' = \frac{s_{i}'}{1 + \beta} = \frac{s_{i} + \beta \cdot s_{i+1}}{1 + \beta} \\ \\\hline& s_{1}'' = \frac{s_{1} + \beta \cdot s_{2}}{1 + \beta} = t_{1} \\& s_{2}'' = \frac{s_{2} + \beta \cdot s_{3}}{1 + \beta} = t_{1}\\& s_{3}'' = \frac{s_{3} + \beta \cdot s_{4}}{1 + \beta} = t_{1} \\& s_{4}'' = \frac{s_{4} + \beta \cdot s_{5}}{1 + \beta} = \frac{t_{1} + \beta \cdot t_{2}}{1 + \beta}\\& s_{5}'' = \frac{s_{5} + \beta \cdot s_{6}}{1 + \beta} = t_{2} \\& s_{6}'' = \frac{s_{6} + \beta \cdot s_{7}}{1 + \beta} = t_{2} \\& s_{7}'' = \frac{s_{7} + \beta \cdot s_{8}}{1 + \beta} = \frac{t_{2} + \beta \cdot t_{3}}{1 + \beta}\\& s_{8}'' = \frac{s_{8} + \beta \cdot s_{9}}{1 + \beta} = t_{3}\\& s_{9}'' = \frac{s_{9} + \beta \cdot s_{10}}{1 + \beta} = t_{4}\\& s_{10}'' = \frac{s_{10} + \beta \cdot s_{11}}{1 + \beta} = t_{5}\\& s_{11}'' = \frac{s_{11} + \beta \cdot s_{12}}{1 + \beta} = t_{6}\\& s_{12}'' = \frac{s_{12} + \beta \cdot s_{13}}{1 + \beta} = t_{7}\\\hline \\ \\\prod_{i\in[n+d-1]} & s_{i}'' = t_{1}^{3}\cdot t_{2}^{2}\cdot t_{3}^{1} \\& \quad\quad \cdot\frac{(t_{1}+\beta t_{2})}{1+ \beta} \cdot\frac{(t_{2}+\beta t_{3})}{1 + \beta}\cdots\cdot\frac{(t_{6}+\beta t_{7})}{1 + \beta} \\ \\\end{align}$$
:::<!-- element style="font-size:0.3em" -->

--
::: left-5
$$\begin{align}& s_{i}''' = \gamma^{-1}(\gamma + s'') = 1 + \gamma^{-1}\cdot s'' = 1 + \gamma^{-1}\frac{s_{i} + \beta \cdot s_{i+1}}{1 + \beta} \\ \\\hline& s_{1}''' = \frac{s_{1} + \beta \cdot s_{2}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{1} \\& s_{2}''' = \frac{s_{2} + \beta \cdot s_{3}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{1}\\& s_{3}''' = \frac{s_{3} + \beta \cdot s_{4}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{1} \\& s_{4}''' = \frac{s_{4} + \beta \cdot s_{5}}{1 + \beta} = (1 + \gamma^{-1})\cdot \frac{t_{1} + \beta \cdot t_{2}}{1 + \beta}\\& s_{5}''' = \frac{s_{5} + \beta \cdot s_{6}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{2} \\& s_{6}''' = \frac{s_{6} + \beta \cdot s_{7}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{2} \\& s_{7}''' = \frac{s_{7} + \beta \cdot s_{8}}{1 + \beta} = (1 + \gamma^{-1})\cdot \frac{t_{2} + \beta \cdot t_{3}}{1 + \beta}\\& s_{8}''' = \frac{s_{8} + \beta \cdot s_{9}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{3}\\ \\& s_{9}''' = \frac{s_{9} + \beta \cdot s_{10}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{4}\\ \\& s_{10}''' = \frac{s_{10} + \beta \cdot s_{11}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{5}\\ \\& s_{11}''' = \frac{s_{11} + \beta \cdot s_{12}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{6}\\ \\& s_{12}''' = \frac{s_{12} + \beta \cdot s_{13}}{1 + \beta} = 1 + \gamma^{-1}\cdot t_{7}\\ \\\hline \\ \\\prod_{i\in[n+d-1]} & s_{i}''' = (1 + \gamma^{-1}\cdot t_{1})^{3}\cdot (1 + \gamma^{-1}\cdot t_{2})^{2}\cdot (1 + \gamma^{-1}\cdot t_{3})^{1} \\& \quad\quad \cdot \frac{(1 + \gamma^{-1})\cdot(t_{1}+\beta t_{2})}{1+ \beta} \cdots \frac{(1 + \gamma^{-1})\cdot(t_{6}+\beta t_{7})}{1 + \beta} \\ \\\end{align}$$
:::<!-- element style="font-size:0.3em" -->
::: right-5


$$\prod_{i\in[n+d-1]} s''' = \prod_{i\in[n]}f' \cdot \prod_{i\in[d-1]}t'$$

and finally compute the rational polynomial which interpolates them
$$Z(\omega^{i}) = \frac{\prod_{j\in[i-1]}f' \cdot\prod_{j\in[i-1]}t'}{\prod_{j\in[n+i]}s'''}$$

And do the grand product check
:::<!-- element style="font-size:0.5em" -->


---

::: title
PLONK + Custom gate
:::
::: left-4
| $X$ | $A(X)$  | $b(X)$   | $C(X)$   |  Gate Info|
| ---| --- | --- | --- | ---|
| $\omega^{0}$ |  1   |  2   |  3   |  $+=0$ |
| $\omega^{1}$ |  1   |  2   |  2   |  $\times = 1$ |
| $\omega^{2}$ |   2  |  3   |  1  |  $\oplus = 2$ |
| $\omega^{3}$ |   0  |  $1 \leftrightarrow A(\omega^{0})$   |  $1 \leftrightarrow A(\omega^{1})$ |   |
| $\omega^{4}$ |   1  |   1  |   2  |  Fib = 3 |
| $\omega^{5}$ |   1  |  2   |  3  |  Fib = 3 |
| $\omega^{6}$ |   2  |  3   |  5 |  Fib = 3 |
| $\omega^{7}$ |   3  |  5   |  8 |  Fib = 3 |
| $\omega^{8}$ |   5  |  8   |  13 |  Fib = 3 |
| $\omega^{9}$ |   8  |  13   |  21 |  Fib = 3 |
:::<!-- element style="font-size:0.4em" -->

::: right-6
1. Addition Gate:<br> $R_{+}(X):=(A(X) + B(X) - C(X))\cdot S(X) = 0$ over $\mathbb{H}_{n}$
   
1. Multiplication Gate:<br> $R_{\times}(X):=(A(X)B(X) - C(X))\cdot S(X) = 0$ over $\mathbb{H}_{n}$
   
1. XOR Gate: <br> $R_{\oplus}(X):=(A(X) + \gamma_{1}B(X) + \gamma_{2}C(X))\cdot S(X) \subset T$ over $\mathbb{H}_{n}$
   
1. Fibonacci Gate(Custom Gate): <br>$$\begin{align} R_{\text{fib}}(X)\\ := & (A(X) + B(X) - C(X)\\ + & \gamma_{1}(A(\omega X) - B(X))\\ + & \gamma_{2}(B(\omega X) - C(X))\end{align}$$ 
   <br> over $\mathbb{H}_{n}$

We can batch gate 0,1,3 & add one  Plookup IOP for gate 2
:::<!-- element style="font-size:0.5em" -->

---

::: title
How to write PLONKish circuit
:::
::: left
- The easiest way: using Chiquito
- or use Halo2 directly
- But the toolings are evolving so fast
- You don't need to write those circuits 
- Programmable Cryptography 
:::
