---
layout: post
category: python,numpy
title: "Orthogonal polynomial regression in Python"
---

Fitting polynomials to noisy data isn't the hippest topic in machine learning these days. A typical machine learning intro course touches on polynomial regression very briefly, as buildup to introducing the kernel trick: given `\(n\)` one-dimensional observations stored as an `\(n \times 1\)`  data vector `\(\v{x}\)`, you *could* construct the [Vandermonde matrix](http://en.wikipedia.org/wiki/Vandermonde_matrix) `\(\v{1} \v{x} \v{x}^2 \ldots \v{x}^m\)`, then just run linear regression -- but we can use this cool kernel trick to avoid actually having to do any of this! 


reasons for orthogonal polynomials:
- powers of x are highly correlated. colinearity leads to unstable coefficients.
- large powers of x are large numbers. so you'll need very small regression coefficients. 

refs: 

http://math.stackexchange.com/questions/279608/how-to-work-out-orthogonal-polynomials-for-regression-model
http://epubs.siam.org/doi/abs/10.1137/0105007
