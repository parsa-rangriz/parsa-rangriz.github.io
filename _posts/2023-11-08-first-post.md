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

## Bethe Free Energy

## BP Algorithm

## BP Fixed Point
