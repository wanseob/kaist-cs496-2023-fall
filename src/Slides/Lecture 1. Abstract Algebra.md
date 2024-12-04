---
defaultTemplate:
  - "[[cs496-2023-fall]]"
---
### KAIST CS496(2023 fall)<br> ZKP - Theory and Applications
Wanseob Lim<br>
PSE, EF

---

#### Lecture 1
#### Unraveling Abstract Algebra and Its Role in Cryptography

---

# Itqwr

note:
- Itqwr
	- Q: What does this mean?
	- A: [[Group]] in Caesar Cipher
- Let's revisit the activities that we had on the 1st day.
	- [[Secret Sharing Scheme]]
	- [[Caesar Cipher]]
	- [[Polynomial Interpolation]]
	- [[Modular Arithmetization]]

--

::: title
#### Caesar Cipher
:::

![](https://hackmd.io/_uploads/HkiYCISN3.png)
![](https://hackmd.io/_uploads/rJWnRIS4n.png)

::: footer
[https://en.wikipedia.org/wiki/Caesar_cipher#/media/File:Caesar_cipher_left_shift_of_3.svg](https://en.wikipedia.org/wiki/Caesar_cipher#/media/File:Caesar_cipher_left_shift_of_3.svg)<br>
https://2023.cs496.sss.codes/ https://www.dcode.fr/caesar-cipher
:::

note:
- Visit https://2023.cs496.sss.codes/
- Decipher
- What's the characteristic of the set of those elements?
- Let's try to express all the elements in Algebraic Math Form
$$
\mathcal{C} = \{A, B, C, \ldots, Z\} = \{y\ |\ y = x \pmod{26} \}
$$

--

::: title
#### Group: usually denoted as $\mathbb{G}$
:::

- Closure:
    - $(a \circ b) \in \mathbb{G}$ for $a,b \in \mathbb{G}$
- Associativity
    - $(a \circ b) \circ c =a \circ (b \circ c)$ for $a,b,c \in \mathbb{G}$
- Identity:
    - $\exists\, e \in \mathbb{G} : e \circ a = a \circ e = a$
- Inverses:
    - $\exists\, a^{-1} \in \mathbb{G} : a \circ a^{-1} = a^{-1} \circ a = e$

note:
- Group is a set
- That satisfies Closure/Associativity/Identity/Inverses about a certain binary operation
- Let's try to find an example: $\mathbb{Z}$

--
::: left
Q: Is ___ a group?
- $(\mathbb{Z}, +)$
- $(\mathbb{Z}, \times)$
- $(\mathbb{Q^*}, \times)$

$\mathbb{Z}$: The set of all integers<br>
$\mathbb{Q^*}$: The set of all non-zero rationale numbers
<!-- element class="small left" -->
:::

note:
- Let's try to write them on the board

--

::: left
Q: Can the alphabets and the Caesar Cipher be called a group?
:::

note:
- Not only the real number can be the group elemement.

--

::: title
#### Caesar Cipher is an additive group
:::

::: left
$$\mathcal{C} = \{A, B, C, \ldots, Z\} = \{y\ |\ y = x \pmod{26} \}$$

If we define the encryption operation as: 

- $a \circ k = a + k \pmod{26}$
- $\text{where} ~ a,k\in \mathcal{C}$

$(\mathcal{C}, \circ)$ is a group

:::

note:
- Let's see what exactly happens

--

::: left
$\mathcal{C}_k = \{y | y = E_k(x) = (x \circ k) = x + k \pmod{26}, \{x, k\} \subset \mathcal{C}\}$
*** 

If $k = H = 7$

$$A + H\pmod{26} = H  \quad J + H\pmod{26} = Q \quad S + H\pmod{26} = Z$$
$$B + H\pmod{26} = I  \quad K + H\pmod{26} = R \quad T + H\pmod{26} = A$$
$$C + H\pmod{26} = J  \quad L + H\pmod{26} = S \quad U + H\pmod{26} = B$$
$$D + H\pmod{26} = K  \quad M + H\pmod{26} = T \quad V + H\pmod{26} = C$$
$$E + H\pmod{26} = L  \quad N + H\pmod{26} = U \quad W + H\pmod{26} = D$$
$$F + H\pmod{26} = M  \quad O + H\pmod{26} = V \quad X + H\pmod{26} = E$$
$$G + H\pmod{26} = N  \quad P + H\pmod{26} = W \quad Y + H\pmod{26} = F$$
$$H + H\pmod{26} = O  \quad Q + H\pmod{26} = X \quad Z + H\pmod{26} = G$$
$$I + H\pmod{26} = P  \quad R + H\pmod{26} = Y \quad A + H\pmod{26} = H$$
::: <!-- element style="font-size: 0.5em" -->

note:
- You can see values are just shifted 7 slots to the right
- It's too easy, right? Let's try to make a difficult version of group

--

::: title
#### A more difficult version using a multiplicative group?
:::

$$\mathcal{C}^* = \{A, B, ..., Z, Å, Ø \}, |\mathcal{C}^*| = 28$$
$$\mathcal{C}_q^* = \{ y\ |\ y = g^{x} \pmod q),  x \in \mathbb{Z}_{\geq 0}\}$$
$\text{ where } q \text{ is a prime number}$

***

And let's define the encryption operation as below:

$$
a \circledast k = g^{a} \cdot g^{k} \pmod{29}$$<br>
$$a,k\in \mathcal{C^*_q}
$$

note:
- Use NodeJS bigint computation, and show the result to students

--

::: title
#### A more difficult version using a multiplicative group?
:::

::: left

$\mathcal{C}_q^* = \{ y\ |\ y = g^{x} \pmod q),  x \in \mathbb{Z}_{\geq 0}\}$<br> $\text{ where } q \text{ is a prime}$

*** 

If $q = 29$ and $g=19$, $k = 0$

$$19^{A}\pmod{29}=S \quad  19^{H}\pmod{29}=Y \quad 19^{O}\pmod{29}=J \quad 19^{V}\pmod{29}=D$$
$$19^{B}\pmod{29}=M \quad  19^{I}\pmod{29}=K \quad 19^{P}\pmod{29}=P \quad 19^{W}\pmod{29}=R$$
$$19^{C}\pmod{29}=O \quad  19^{J}\pmod{29}=F \quad 19^{Q}\pmod{29}=N \quad 19^{X}\pmod{29}=W$$
$$19^{D}\pmod{29}=X \quad  19^{K}\pmod{29}=Å \quad 19^{R}\pmod{29}=E \quad 19^{Y}\pmod{29}=B$$
$$19^{E}\pmod{29}=U \quad  19^{L}\pmod{29}=T \quad 19^{S}\pmod{29}=H \quad 19^{Z}\pmod{29}=I$$
$$19^{F}\pmod{29}=V \quad  19^{M}\pmod{29}=C \quad 19^{T}\pmod{29}=G \quad 19^{Å}\pmod{29}=Z$$
$$19^{G}\pmod{29}=L \quad  19^{N}\pmod{29}=Ø \quad 19^{U}\pmod{29}=Q \quad 19^{Ø}\pmod{29}=A$$
:::<!-- element style="font-size: 0.5em" -->

note:
- Use NodeJS bigint to compute the result

--

::: title
### Generator
:::

Generators for additive group $\mathbb{G}$ satisfy

 $$n \cdot G \in \mathbb{G}$$

Generators for multiplicative group $\mathbb{G}^*$ satisfy:<br>
  $$g^{n} \in \mathbb{G^*}$$
note:
Q: What if $g$ is 3 or 5? in the previous case.

--

### $\mathbb{Z}_q$ & $\mathbb{Z}_q^*$

***

- $\mathbb{Z}_q$: Additive group of integers modulo q
- $\mathbb{Z}_q^*$: Multiplicative group of integers modulo q

note:
- The set of integers are additive group
- The set of non-zero integers are multiplicative group
- They are finite group

--

Using $\mathbb{Z}_q$ (Caesar Cipher)

Olssx Dvysk?

***

Using $\mathbb{Z}_q^*$ (Multiplicative Cipher)

ÅSKHG?

--

It is hard to know $x$ using $y$ when $y = g^{x} \pmod p$

***

This is called the ***Discrete Logarithm Problem***

--

What can we do w/ the group theory & ***Discrete Logarithm Problem***

--

Questions: What is _____ ?
- Group?
- $\mathbb{Z}$?
- $\mathbb{Z_q}$?
- $\mathbb{Z^*_q}$?
- Generator?
- Are they cyclic?

note:
- Group should have its set and the definition for the binary operation
- What's the difference between $Z$ and $Z_q$?
- $\mathbb{Z}$ is the set of all integers which is also a group against addition
- Is $\mathbb{Z_q}$ finite?
- What's the difference between $\mathbb{Z}^*_q$ and $\mathbb{Z}_q$?
- What's the difference between $\mathbb{Z}^*_q$ and $\mathbb{Z}_q$?
- What's the difference between $\mathbb{Z}^*_q$ and $\mathbb{Q}^*$?

---

### RSA cryptogrpahy

***

<span style="font-size:0.5em">

1. Key generation
    - choose two large $p,q$
    - compute $n = p*q$
    - compute the Euler's Phi function $\phi(n) = (p-1)*(q-1)$
    > Euler's Phi Function returns the number of all coprimes to $n$ in $[n]$
    - Choose an integer $e$ such that $1<e<\phi(n)$ and $e$ is coprime to $\phi(n)$
    > Usually choose 65537 or 3 for the encyrption performance
    - Compute the modular inverse of $e$ which is denoted as $d$ which satisfies $e*d \equiv 1 \pmod{\phi(n)}$
    - Public key pair is $(n, e)$ and the private key pair is $(n, d)$
    
</span>

--

### RSA cryptogrpahy

***

<span style="font-size:0.5em">

2. Encryption & Decryption
    - Encrypting: Ciphertext $c$ of a message $m$ is $c \equiv m^e \pmod n$
    - Decrypting: Plaintext $m$ of a ciphertext $c$ is $m \equiv c^d \pmod{n}$
    
> - By the Euler's theorem, if $a$ and $n$ are relatively prime,
>   $a^{\phi(n)} \equiv 1 \pmod{n}$
> - Since $e\cdot d \equiv 1 \pmod{\phi(n)}$, there exists an integer $k$ such that $e\cdot d = k \cdot \phi(n) + 1$.
> - Therefore, $c^d \equiv m^{ed} \equiv m^{1 + k\phi(n)} \pmod{n} \equiv m \cdot m^{k\phi(n)} \equiv m \pmod{n}$
    
</span>

--

### RSA cryptogrpahy

***

<span style="font-size:0.5em">

3. Digital Signature for a hash value $h$
    - Signing: signature $s \equiv h^{d} \pmod n$ 
    - Verification: $s^e \equiv h^{ed} \equiv h \pmod n$
    
</span>

--

RSA is secure when its key is larger than 2048 bits

***

ECC(Elliptic Curve Cryptography) can achieve the same security as a 3072-bit key in RSA

note:
RSA depends on factoring problem which is called RSA assumption
ECC depends on DLA

---

### Elliptic Curve Cryptography

--

### Field

--

### Abelian Group

***

\+ Commutativity
- $a,b \in \mathbb{G}$
- $a \circ b = b \circ a$

--

### Field: usually denoted as $\mathbb{F}$
***
- has 2 binary operations ($+$, $\times$)
- $\{ x | x \in \mathbb{F}\}$ is an **abelian group** for $+$
- $\{ x | x \in \mathbb{F}, x \neq 0 \}$ is an **abelian group** for $\times$
- The distributive law: $a\times(b+c) = a\times b + a\times c$

--

$\mathbb{G}$ is an additive group that is a set of all points on an elliptic curve defined on a finite field $\mathbb{F_p}$ where $p$ is a large prime

--

For a group $\mathbb{G}$, we can define the additive binary operation using geometry

***

When a line intersects three points on the curve
- $P + Q + R = 0$
- $P + Q = - R$

https://andrea.corbellini.name/2015/05/17/elliptic-curve-cryptography-a-gentle-introduction/

--

## EC-DLA

--

In the multiplicative group, for the DLA
- It is hard to know $x$ using $y$ when $y = g^x$
***
In the additive group, for the DLA
- It is hard to know $x$ using $y$ when $y = xG$

--

#### How to compute $xG$ (Scalar Multiplication) in ECC?
***
- Double & Add
$$13G = (1*2^{3} + 1*2^{2} + 0*2^{1} + 1*2^{0}) G$$

--

### DH<br>(Diffie Hellman Key Exchange)

- $A = g^a, B = g^b$ where $A,B$ are the public keys and $a,b$ are the private keys
- Shared key $k = g^{ab}$
***

### ECDH<br>(Elliptic Curve DH)

- $A = aG, B = bG$ where $A,B$ are the public keys and $a,b$ are the private keys
- Shared key $k = abG$

--

::: title
### ELGAMAL on DLA
:::

<span style="font-size:0.7em">

- Alice & Bob's key pairs: $A = g^{a}$ and $B = g^{b}$
- Alice chooses a random integer $k \in [p-2]$
- Compute the ciphertext pairs $(c_1, c_2)$ as follows
    - $c_1 = g^{k} \pmod{p}$
    - $c_2 = m*(B^{k}) \pmod{p}$
- Bob now decrypts the message $(c_1, c_2)$ as follows:
    - $m = c_2 * (c_1^{b})^{-1} \pmod{p}$
    > $$= c_2 * (c_1^{b})^{-1} \pmod{p}$$
    > $$= m*(B^{k}) * ((g^{k})^{b})^{-1}$$
    > $$= m*(g^{bk}) * (g^{bk})^{-1}$$
    > $$= m$$
    
</span>

--

::: title
### ELGAMAL on ECDLA
:::

<span style="font-size:0.7em">

- Encode message m into $M$
- Alice & Bob's key pairs: $A = aG$ and $B = bG$
- Alice chooses a random integer $k \in [p-1]$
- Compute the ciphertext pairs $(C_1, C_2)$ as follows
    - $C_1 = k*G$
    - $C_2 = M + k*B$
- Bob now decrypts the message $(c_1, c_2)$ as follows:
    - $M' = C_2 - b * C_1$

</span>

--

::: title
### Schnorr Signature
:::

::: left 
Given parameters:
- private key: $k$
- public key: $P$
- challenge: $e$

***

What if the signature is $s = ek$?<br>
It's very easy to retrieve $k$ from $k = s/e$

***

Instead
- we choose a random nonce $r$
- And share its public nonce $R = rG$ together
- The signature becomes $(s, R)$ where $s = r + ke$
:::<!-- element style="font-size: 0.7em" -->

--

::: title
### ECDSA
:::

- public key & private key: $P = pG$
- Choose a random nonce $k$
- Its public nonce: $r = (kG).x$
- $s = {1 \over k}(e + p\cdot r)$
- Signature: $(r, s)$
- Verification: ${e \over s}G + {r \over s}P = ({e + rp \over s})G = kG$


