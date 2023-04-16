---
title: "RSA Review"
date: 2023-04-16T11:41:08-04:00
draft: false
---

Here are some notes on how RSA works. I mainly wrote them up as a review for myself. They are mostly from the RSA paper (which you should probably go read instead).

Let

- $p$, $q$ be two primes
- $n = pq$
- $M$: an integer, the message, where $0 \leq M < n$
- $E$ and $D$: the encryption and decryption algorithms
- $C$: cyphertext.

Pick a large, random integer $d$ relatively prime to $\phi(n)$, where $\phi$ is the Euler totient function.
$\phi$ is a multiplicative function, so $\phi(xy)=\phi(x)\phi(y)$. Also, if $x$ is prime, $\phi(x) = x-1$.

In modular arithmetic, if $a$ and $m$ are relatively prime, $a \pmod m$ has a modular inverse, i.e. there exists an $x$ such that $ax \equiv 1 \pmod m$. Thus $d$ has a modular inverse $e$ such that $ed \equiv 1 \pmod {\phi(n)}$.

$C$, $E$, and $D$ are defined as follows:

$$
\begin{aligned}
C \equiv & E(M) \equiv M^e \pmod n \\\\
         & D(C) \equiv C^d \pmod n
\end{aligned}
$$

Now we show that $D(E(M))$ yields $M$.

$$
\begin{aligned}
D(C) \equiv D(M^e) \equiv (M^e)^d \equiv M^{ed} &\equiv M^{1 \pmod {\phi(n)}} \pmod n \\\\
                                                &\equiv M^{k\phi(n)+1} \pmod n
\end{aligned}
$$

since $1 \pmod {\phi(n)}$ is $k\phi(n) + 1$ in $\mathbb Z$ for some integer $k$ by definition.

Euler's theorem states that if $a$ and $n$ are relatively prime, $a^{\phi(n)} \equiv 1 \pmod n$.

When $p$ does not divide $M$, since $p$ is prime, $p$ and $M$ are relatively prime. Thus, by Euler's theorem, we have

$$M^{p-1} \equiv 1 \pmod p$$

Since $p-1$ divides $\phi(n)$, we have

$$M^{k\phi(n)+1} \equiv M^{l(p-1)}M \equiv 1 M \equiv M \pmod p$$

for some $l \in \mathbb Z$. But note that when $M$ divides $p$, $M \equiv 0 \pmod p$, and so

$$M^{k\phi(n)+1} \equiv M \pmod p$$

always holds, regardless of whether $p$ divides $M$ or not. Arguing likewise for $q$, we get

$$M^{k\phi(n)+1} \equiv M \pmod q$$

By the Chinese remainder theorem, if there is a solution to

$$
\begin{align}
x \equiv a \mod p \\\\
x \equiv b \mod q
\end{align}
$$

then the solution is unique modulo $pq$. That is, there is only one solution $x$, and it is an integer such that $1 \leq x < pq$.

Thus, we have

$$M^{k\phi(n)+1} \equiv M \pmod {pq}$$

and so

$$D(C) \equiv M \pmod n$$

# References

- [A Method for Obtaining Digital Signatures and Public-Key Cryptosystems (Rivest, Shamir, & Adleman)](https://people.csail.mit.edu/rivest/Rsapaper.pdf) (the RSA paper)
- [zkSNARKs in a nutshell - Ethereum Foundation Blog](https://blog.ethereum.org/2016/12/05/zksnarks-in-a-nutshell)
- [Euler's theorem - Wikipedia](https://en.wikipedia.org/wiki/Euler%27s_theorem)
- [Modular multiplicative inverse - Wikipedia](https://en.wikipedia.org/wiki/Modular_multiplicative_inverse)
- [The Chinese Remainder Theorem (Keith Conrad)](https://kconrad.math.uconn.edu/blurbs/ugradnumthy/crt.pdf)
