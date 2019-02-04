---
published: true
title: Computability Theory - Introduction
use_math: true
category: Math
layout: default

---

# What is Computability Theory?

*Computability theory* is a branch of mathematics that attempts to analyze and categorize problems into ones that can be decideable or computable under some given restrained resources. These resources are fairly applicable to real life scenarios, i.e. memory or time. I've had the privilege of learning from Yiannis N. Moschovakis and Amit Sahai, who are both very knowledgeable in this field. Here I attempt to condense some of the material into digestible readings for those who want to get a brief introduction. It does require a bit of mathematical background as I will be assuming that you know basic proofs by induction and some notation. 

# Recursion and Primitive Recursive Functions 

It is intuitive enough that in order to find out what our computers can do, we start with induction and recursion. For loops and otherwise complicated programs have some type of implicit recursion. It turns out that the **principle of mathematical induction** and the **basic recursion lemma** are almost synonymous (one can prove induction given recursion, and vice versa).

The **basic recursion lemma**(a result of Kleene's second recursion theorem) is stated as follows: For all sets $X,W$, and any given functions $g:X\to W, h:W \times \mathbb{N} \times X \to W$, there exists one **unique** function $f:\mathbb{N} \times X \to W$ such that:

$$f(0,x) = g(x), \\ f(n+1,x) = h(f(n,x), n, x)$$

Basically, $f$ can be recursively defined by some "generating function" $h$ and some "base function" $g$. **All of the interesting functions we can compute on our computers are recursive in nature.**

There is a specific class of recursive functions, called **primitive recursive**, denoted as $\mathcal{R}_p$. Roughly speaking, it is the set of functions that are defined by:

1. Constant functions are in $\mathcal{R}_p, C^n_q(x_1,...,x_n) = q$
2. The projection functions are in $\mathcal{R}_p, P^n_i(x_1,...,x_i,...,x_n) = x_i$.
3. The successor functions are in $\mathcal{R}_p, S(x) = x + 1$.
4. Any composition of (1-3) are also in the set.
5. If $g, h \in \mathcal{R}_p$, and $f$ is defined as: $$f(0,x) = g(x), \\ f(y+1, x) = h(f(y,x),y,x)$$, then $f \in \mathcal{R}_p$. 

We can build addition, multiplication, exponentiation, if-statements, loops, etc from this set of functions. Most of the functions we use in computer science can be expressed as some $f \in \mathcal{R}_p$.

An example of a recursive function that is **not primitively recursive** is the **Ackermann function**, which we will discuss in detail later:

$$A(0,x) = x + 1, \\ A(n+1, 0) = A(n,1), \\ A(n+1, x+1) = A(n, A(n+1, x))$$

It is not clear at first that $A$ has a unique solution; the proof is done via *double induction*. We first suppose there exists $f, g$ which are solutions to $A$ for some inductively increasing subdomain, and then we show that they must be equal.

- **Sub-induction**: $A(0, 0) = 1$. Then $f(0,0) = g(0,0) = 1$. And it is clear that $\forall x, f(0,x+1) = g(0,x+1) = x+2$, so they are unique.
- **Base**:  Suppose $\forall x, A(n, x)$ has a unique solution $\implies A(n+1, 0)$ unique as well. This is true since $A(n+1, 0) = A(n, 1)$, which we know is unique, so $f(n+1,0) = g(n+1,0) = f(n,1)$.
- **Inductive**: Suppose $\forall n \geq 1, \forall x, A(n-1,x)$ has a unique solution, as well as $A(n, x)$ by the inductive hypothesis. Then, $A(n, x+1) = A(n, A(n+1, x)) = f(n, f(n+1, x)) = g(n, g(n+1, x))$ must be unique.
