---
layout: post
category: inference
title: "A Taxonomy of Variational Inference Methods"
---

INSERT CHATTY INTRO HERE

Types of variational inference:

I'll consider a general case where we have a joint distribution $p(Z,X)$ over observed variables $X$ and hidden variables $Z$, we want to compute $p(Z|X)$, and to do this we will use an approximating distribution $q(Z)$. Note that $X$ and $Z$ can in general be large sets of random variables each with their own complicated internal structure (e.g. in an HMM, $Z = {Z_1, Z_2, \cdot, Z_T}$ might denote the hidden state variables, which follow the conditional independence relationships implied by the Markov chain structure). 

- *Factorized approximations.* These assume that $q(Z)$ factors in a particular way, for example, $q(Z)=q_A(Z_A)q_B(Z_B)$ for two subsets $A$ and $B$. 

- *questions for me*:
- when is this assumption sufficient to determine the functional form of $q$?

 - special cases:
   - Mean-field approximations. These assume a *total* factorization; that is, $q(Z) = \prod_{i=1}^N q_i(Z_i)$.
   - Variational message passing

- *Local approximations.* 

- *Expectation propagation*

- *whatever BP is/does*. in some sense this is a type of expectation propagation...



thoughts:
- we can separate what the variational objective is / how it's derived -- which concerns things like factorized / local approximations -- from questions about how we optimize it, which opens up the difference between closed-form coordinate ascent, stochastic variational inference, etc. 
- things it would be nice to mention: black-box variational inference. streaming variational bayes. 
- what's the role of exponential families here?
- relation to EM
- when people say that variational inference is "approximate", don't be too discouraged: true posteriors are complicated creatures and we generally don't know what to do with them anyway. Often people just try something like MAP which is a degenerate form of variational inference anyway. 
