---
title: 'Belief Propagation'
date: 2023-11-08
permalink: /posts/2012/08/blog-post-4/
tags:
  - algorithms
  - statistical physics
  - random graphs
---
## Introduction
Let $(\Omega^n, \mathscr A^n, \mu)$ be a probability space such that $\mu \in \mathcal P$ where $\mathcal P$ is a family of probability measures defines on $\mathscr A^n$. Also let $X = (X_1, \cdots, X_n)$ be a random vector so that $X_i: \Omega \to \mathbb R$ is a measurable function for all $i = [n]$. Then the marginal probability of $X$ is defines as 

$$\mu_i(X_i) := \mu(X | \mathscr A_i) = E(I_{X_i}|\mathscr A_i)$$

Now the main problem is to compute marginals $\mu_i(X_i)$ when the measure $\mu$ is unkown, but we know the Hamiltonian, or so called facotrization functions.

## Gibbs Free Enegry
Let $f: \Omega^n \to \mathbb R^+$ be a non-negative real function such that for any probability measure $\mu \in \mathcal P$ we can write, 

$$\mu(x) = \frac{1}{Z} f(x)$$

where $Z = \sum_{X \in \Omega^n} f(x)$ is called the partition function. Then, the marginal of $x_I$ where $I$ is an arbitrary index of $[n]$, is denoted by

$$\mu_I(x_I) := \sum_{x_{[n] \setminus I}} \mu(x)$$

Thus, the Shannon entropy, $H: \mathcal P \to \mathbb R$ is given by

$$H(\nu) = \sum_{x \in \Omega^n} \nu(x) \log \frac{1}{\nu(x)}$$

Remark: In physics, $\mu \mapsto \mu_\beta$ and $Z \mapsto Z_\beta$ due to their dependence on the inverse temperature $\beta$. Also the quantity $-\frac{1}{\beta}\log Z$ is called the free energy but in inference problems $\beta$ is always 1.

Moreover, the Gibbs free energy, $G: \mathcal P \to \mathbb R$, is defined as follows,

$$G(\nu) := \sum_{x \in \Omega^n} \nu(x) \log \frac{1}{f(x)} - H(\nu)$$

However, one may write the Gibbs free energy in a form such that it is the summation of free energy and KL-divergence (relative entropy),

$$G(\nu) = \sum_{x \in \Omega^n} \nu(x) \log \frac{1}{Z \mu(x)} - \sum_{x \in \Omega^n} \nu(x) \log \frac{1}{\nu(x)} = -\log Z + D(\nu \| \mu)$$

Hence since $D(\nu \| \mu) \geq 0$ for any $\mu, \nu \in \mathcal P$, with equality iff $\mu = \nu$ then the minimum of the Gibbs free energy is the free energy, i.e., $$\min_{\nu \in \mathcal P} G(\nu) = -\log Z$$

## Factor Graph
A factor graph is a bipartite graph $G = (V, F, E)$ such that $V$ (variable nodes) and $F$ (factor nodes) are two disjoint finite sets of vertices and $E \subseteq V \times F$ a set of edges. Also let,

$$V(a) := \{i \in V: (i, a) \in E\}, \quad \forall a \in F$$

be the variable nodes adjacent to factor node $a$.

Similarly,

$$F(i) := \{a \in F: (i, a) \in E\}, \quad \forall i \in V$$

The joint distribution $\mu$ over $x \in $\Omega^n$ factors on the factor graph $G$ if there exists a vector of functions $\hat f = (\hat f_a: a \in F)$ where $\hat f_a: \Omega^{|V(a)|} \to \mathbb R^+$ and $f = (f_i: i \in V)$ where $f_i: \Omega \to \mathbb R^+$ such that

$$\mu(x) = \left( \prod_{a \in F} \hat f_a(x_{V(a)})\right) \left( \prod_{i \in V} f_i(x_i)\right)$$.

**Lemma 1:** Let $A \subseteq F$ whose induced subgraph is connected, and let $V(A) := \cup_{a \in A} V(a)$. Then the marginal $\mu_{V(A)}$ can be written as 

$$\mu_{V(A)} (x_{V(A)}) = \left( \prod_{a \in F} \frac{\mu_{V(a)}(x_{V(A)})}{\prod_{i \in V(a)} \mu_i(x_i)}\right) \left( \prod_{i \in V(a)} \mu_i (x_i) \right)$$

 **Lemma 2:** The entropy of $\mu$ of the factor graph denoted
 
 $$H(\mu) = \sum_{a \in F} H(\mu_{V(A)}) - \sum_{i \in V} |F(i) -1| H(\mu_i)$$
 
 and the free energy is given by
 
 $$-\log Z = -H(\mu) + \sum_{a \in A} \left( \sum_{x_{V(a)}} \mu_{V(a)}(x_{V(a)}) \log \frac{1}{\hat f_a(x_{V(a)}})\right) + \sum_{i \in V} \left( \sum_{x_i} \mu_i(x_i) \log \frac{1}{f_i(x_i)} \right)$$

## Bethe Free Energy
Consider a set of variable node beliefs $b := \{b_i : \Omega \to [0, 1], \quad i \in V\}$ and factor node beliefs $\hat b = \{\hat{b}_a: \Omega^{|V(a)|} \to [0, 1], a \in F\}$ satisfying the marginal consistency contraint,

$$\sum_{x_{V(a) \setminus i}} \hat b_a (x_{V(a)}) = b_i (x_i)$$

for all $(i, a) \in E$ and $x_i \in \Omega$. Also, $\mathcal M = \{(b, \hat b)\}$ is called a marginal polytope.

For any $(b, \hat b) \in \mathcal M$, T\the Bethe free energy $H_B: \mathcal M \to \mathbb R^+$ is defined to be

$$H_B(b, \hat b) := \sum_{a \in F} \hat b_a(x_{V(a)}) \log \frac{1}{\hat b_a(x_{V(a)})} - \sum_{i \in V} \left( |F(i)| - 1 \right) \sum_{x_i} b_i(x_i) \log \frac{1}{b_i(x_i)}$$

which is similar to the free energy .

## BP Algorithm

## BP Fixed Point
