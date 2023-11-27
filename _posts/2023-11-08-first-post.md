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

Hence since $D(\nu \| \mu) \geq 0$ for any $\mu, \nu \in \mathcal P$, with equality iff $\mu = \nu$ then the minimum of the Gibbs free energy is the Gibbs entropy, i.e., $$\min_{\nu \in \mathcal P} G(\nu) = -\log Z$$

## Factor Graph
A factor graph is a bipartite graph $G = (V, F, E)$ such that $V$ (variable nodes) and $F$ (factor nodes) are two disjoint finite sets of vertices and $E \subseteq V \times F$ a set of edges. Also let,

$$V(a) := \{i \in V: (i, a) \in E\}, \quad \forall a \in F$$

be the variable nodes adjacent to factor node $a$.

Similarly,

$$F(i) := \{a \in F: (i, a) \in E\}, \quad \forall i \in V$$

The joint distribution $\mu$ over $x \in \Omega^n$ factors on the factor graph $G$ if there exists a vector of functions 

$$\hat f = (\hat f_a: a \in F) \text{ where } \hat f_a: \Omega^{|V(a)|} \to \mathbb R^+, \quad f = (f_i: i \in V) \text{ where } f_i: \Omega \to \mathbb R^+$$ 

such that $\mu(x) = \left( \prod_{a \in F} \hat f_a(x_{V(a)})\right) \left( \prod_{i \in V} f_i(x_i)\right)$.

**Lemma 1:** Let $A \subseteq F$ whose induced subgraph is connected, and let $V(A) := \cup_{a \in A} V(a)$. Then the marginal $\mu_{V(A)}$ can be written as 

$$\mu_{V(A)} (x_{V(A)}) = \left( \prod_{a \in F} \frac{\mu_{V(a)}(x_{V(A)})}{\prod_{i \in V(a)} \mu_i(x_i)}\right) \left( \prod_{i \in V(a)} \mu_i (x_i) \right)$$

 **Lemma 2:** The entropy of $\mu$ of the factor graph denoted
 
 $$H(\mu) = \sum_{a \in F} H(\mu_{V(A)}) - \sum_{i \in V} |F(i) -1| H(\mu_i)$$
 
 and the Gibbs entropy is given by
 
 $$-\log Z = -H(\mu) + \sum_{a \in A} \left( \sum_{x_{V(a)}} \mu_{V(a)}(x_{V(a)}) \log \frac{1}{\hat f_a(x_{V(a)})}\right) + \sum_{i \in V} \left( \sum_{x_i} \mu_i(x_i) \log \frac{1}{f_i(x_i)} \right)$$

## Bethe Free Energy
Consider a set of variable node beliefs $\tau := \{\tau_i : \Omega \to [0, 1], i \in V\}$ and factor node beliefs $\hat \tau = \{\hat{\tau}_a: \Omega^{|V(a)|} \to [0, 1], a \in F\}$ satisfying the marginal consistency contraint,

$$\sum_{x_{V(a) \setminus i}} \hat \tau_a (x_{V(a)}) = \tau_i (x_i)$$

for all $(i, a) \in E$ and $x_i \in \Omega$. Also, $\mathcal M = \{(\tau, \hat \tau)\}$ is called a marginal polytope.

For any $(\tau, \hat \tau) \in \mathcal M$, the Bethe entropy $H_B: \mathcal M \to \mathbb R^+$ is defined to be

$$H_B(\tau, \hat \tau) := \sum_{a \in F} \hat \tau_a(x_{V(a)}) \log \frac{1}{\hat \tau_a(x_{V(a)})} - \sum_{i \in V} \left( |F(i)| - 1 \right) \sum_{x_i} \tau_i(x_i) \log \frac{1}{\tau_i(x_i)}$$

which has the similar form with the Gibbs entropy (Lemma 2) but in a different domain.

Moreover, the Bethe free energy is defined as

$$F_B(\tau, \hat \tau) := \sum_{a \in F} \sum_{x_{V(a)}} \hat \tau_a(x_{V(a)}) \log \frac{1}{f_a(x_{V(a)})} + \sum_{i \in V} \sum_{x_i} \tau_i(x_i) \log \frac{1}{f_i(x_i)} - H_B(\tau, \hat \tau)$$

## BP Algorithm
Now we aim to find an optimal point $\tau^* = \arg \min_{\tau \in \mathcal M} F_B(\tau)$. To find this we need to use the Lagrangian multipliers:

$$L_B(\tau) = F_B(\tau) + \sum_{i \in V} \lambda_i \left(\sum_{x_i} \tau_i(x_i) - 1\right) + \sum_{a \in F} \hat \lambda_a \left(\sum_{x_{V(a)}} \hat \tau_a(x_{V(a)}) - 1\right) + \sum_{(i, a) \in E} \sum_{x_i} \rho_{ia}(x_i) \left(\sum_{x_{V(a) \setminus i}} \hat \tau_a(x_{V(a)}) - \tau_i(x_i)\right)$$

Stable point conditio $(i, a) \in E$:

$$0 = \frac{\partial{L_B}}{\partial \tau_i} = -\log \frac{1}{f_i(x_i)} + (1 - |F(i)|) (1 + \log \tau_i(x_i)) + \lambda_i - \sum_{a \in F(i)} \rho_{i, a}(x_i)$$

$$0 = \frac{\partial L_B}{\partial \hat \tau_a} = -\log \frac{1}{\hat f_a(x_{V(a)})} + (1 + \log \hat \tau_a(x_{V(a)}) + \hat \lambda_a + \sum_{i\in V(a)} \rho_{i, a} (x_i)$$

Then for all $i \in V$ and $a \in F$, 

$$\tau_i^* (x_i) = \exp\left(\frac{1}{1-d_i} \left(\sum_{a \in F(i)} \rho_{i, a} (x_i) + \log \frac{1}{f_i(x_i)} - \lambda_i\right) - 1 \right)$$

$$\hat \tau_a^*(x_{V(a)}) = \frac{1}{\hat f_a(x_{V(a)})} \exp \left( -\lambda_a - \sum_{i \in V(a)} \rho_{i, a}(x_i) - 1 \right)$$

For factor graphs with special structures like trees, a dynamic programming approach allows linear time computation of marginals. This recursive process can be interpreted as a distributed message-passing algorithm, known as "belief propagation".

For each edge $(i, a) \in E$ we define two messages $\tau_{i \to a}: \Omega \to [0, 1]$ and $\hat \tau_{a \to i}: \Omega \to [0, 1]$ as follows:

$$\tau_{i \to a}(x_i) := \frac{1}{Z_{i \to a}} \prod_{c \in F(i) \setminus a} \tau_{c \to i}(x_i)$$

$$\hat \tau_{a \to i}(x_{V(a)}) := \frac{1}{\hat Z_{a \to i}} \frac{1}{f_i(x_i)} \exp(-\rho_{i, a}(x_i))$$

where $Z_{i \to a} = \sum_{x_i} \tau_{i \to a}(x_i)$ and $\hat Z_{a \to i} = \sum_{x_i} \hat \tau_{a \to i}(x_i)$ for all $(i, a) \in E$.

Hence, 

$$\tau_i^*(x_i) = \overbrace{\exp \left( \frac{\lambda_i}{d_i - 1} - 1 \right)}^{=: A} \exp\left(-\sum_{a \in F(i)} \rho_{i, a}(x_i) + \log f_i(x_i) \right)^{\frac{1}{d_i - 1}}$$

## BP Fixed Point

