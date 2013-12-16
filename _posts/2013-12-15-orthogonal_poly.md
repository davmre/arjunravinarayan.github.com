---
layout: post
category: python
title: "Orthogonal polynomial regression in Python"
---

*tl;dr: I ported an R function to Python that helps avoid some numerical issues in polynomial regression.*

Fitting polynomials to data isn't the hottest topic in machine learning. A typical machine learning intro course touches on polynomial regression only as a foil to the kernel trick: given a column vector `$\v{x}$` consisting of `$n$` one-dimensional observations, you *could* construct the [Vandermonde matrix](http://en.wikipedia.org/wiki/Vandermonde_matrix) `\[V = \left[\v{1}, \v{x}, \v{x}^2, \cdots, \v{x}^m\right],\]`which represents your data by polynomial features, and then just treat `$V$` as the design matrix for a linear regression. But this gets unwieldy if you want to use a high-degree polynomial, or if you have multiple variables interacting, so instead you can use the kernel trick to avoid ever having to build `$V$`! And once you've kernelized everything, you might as well just ignore polynomials altogether and go straight for something like an RBF kernel. 

That's all great, but sometimes you really do just want to do polynomial regression. If you have a lot of data points that appear to follow a simple nonlinear function, a low-degree polynomial is going to give you a much more compact, efficient representation of that function than what you'd get from a kernel method. 

Unfortunately, although the naive approach to polynomial regression works fine for trivial examples, some issues can pop up in practice. Powers of `$\v{x}$` are correlated, and regression on correlated predictors [leads to unstable coefficients](http://en.wikipedia.org/wiki/Multicollinearity#Consequences_of_multicollinearity): the coefficients from an order-3 polynomial regression might change drastically when moving to an order-4 regression. Furthermore, if `$x$` takes large values, then powers of `$\v{x}$` will grow quite large and the normal equations matrix `$V^TV$` will be poorly conditioned. A related issue is that large powers of `$\v{x}$` are very large numbers, so will require very small regression coefficients, potentially leading to underflow and coefficients that are hard to interpret or compare intuitively. 

The standard fix to these issues is to use an *orthogonal* polynomial basis [^1]. That is, instead of the standard monomial basis `\[\left[\v{1}, \v{x}, \v{x}^2, \cdots, \v{x}^m\right],\]` we use some other basis `\[\left[\v{b}_1, \v{b}_2, \v{b}_3, \cdots, \v{b}_4\right],\]` where the vectors `$(\v{b}_i)$` span the same subspace as the 'monomial' vectors, but also form an orthogonal basis for that subspace, meaning that for all `$i\ne j$` we have `$\v{b}_i^T\v{b}_j = 0$`. The change of basis is just a reparameterization, so it doesn't affect the final regression function, but now we have a set of *uncorrelated* predictors, and `$V^TV$` is trivial to invert since it's just a diagonal matrix! 

How can we find an orthogonal polynomial basis for a given dataset? If you're a statistician, you just use R's [built-in](http://stat.ethz.ch/R-manual/R-patched/library/stats/html/poly.html) `poly()` method, but unfortunately there doesn't seem to be any official equivalent in the Python ecosystem. So I checked out the [source](https://svn.r-project.org/R/tags/R-3-0-2/src/library/stats/R/contr.poly.R); it turns out that R just uses a [QR decomposition](http://en.wikipedia.org/wiki/QR_decomposition), i.e., it decomposes`\[V = QR\]`where `$Q$` is an `$n \times n$` orthogonal matrix whose columns form an orthogonal basis for the same `$m$`-dimensional subspace spanned by the columns of `$V$`. The matrix `$R$`, which tells us how to "reconstruct" `$V$` from `$Q$`, is upper triangular, which guarantees that the first `$k$` columns of `$V$` can be represented as a linear combination of the first `$k$` columns of `$Q$`. Remember that the first `$k$` columns of `$V$` are just the monomials up to order `$k-1$`, so this means that the first column of `$Q$` must be a constant, the second linear (w.r.t. `$\v{x}$`), and so on. This means that we can take the columns of `$Q$` to be our orthogonal basis, without losing any of the intuitive 'polynomial-ness' of the original basis.

I've ported the `poly()` method from R to Python/numpy, and am posting it here in the hope that someone else might find this useful. Diverging slightly from the R version, I've split the code into two separate functions. The first, `ortho_poly_fit`, takes as input a vector `x` and a polynomial degree, and returns a matrix `Z` containing an orthogonal polynomial representation of `x`, along with extra coefficent vectors `norm2` and `alpha`. You can use `Z` as a drop-in replacement for the Vandermonde matrix, i.e. just plug it in to your favorite linear regression package to estimate polynomial coefficients from data. The additional outputs `norm2` and `alpha` are used by the second function `ortho_poly_predict`, which is used at test time to convert a *new* vector of inputs into the same basis that was found for the training data. Note that there's no guarantee of orthogonality for the test points -- the basis we found was specific to the training points -- but it should be pretty close as long as the test points are from roughly the same range as the training data. 

{% highlight python %}
import numpy as np

def ortho_poly_fit(x, degree = 1):
    n = degree + 1
    x = np.asarray(x).flatten()
    if(degree >= len(np.unique(x))):
            stop("'degree' must be less than number of unique points")
    xbar = np.mean(x)
    x = x - xbar
    X = np.fliplr(np.vander(x, n))
    q,r = np.linalg.qr(X)

    z = np.diag(np.diag(r))
    raw = np.dot(q, z)

    norm2 = np.sum(raw**2, axis=0)
    alpha = (np.sum((raw**2)*np.reshape(x,(-1,1)), axis=0)/norm2 + xbar)[:degree]
    Z = raw / np.sqrt(norm2)
    return Z, norm2, alpha

def ortho_poly_predict(x, alpha, norm2, degree = 1):
    x = np.asarray(x).flatten()
    n = degree + 1
    Z = np.empty((len(x), n))
    Z[:,0] = 1
    if degree > 0:
        Z[:, 1] = x - alpha[0]
    if degree > 1:
      for i in np.arange(1,degree):
          Z[:, i+1] = (x - alpha[i]) * Z[:, i] - (norm2[i] / norm2[i-1]) * Z[:, i-1]
    Z /= np.sqrt(norm2)
    return Z
{% endhighlight %}
 
[^1]: For more on regression with orthogonal polynomials, see the lecture notes by [Geaghan](http://www.stat.lsu.edu/faculty/geaghan/exst7034/fall2005/pdf/17-polynomials.pdf) and [Keles](ftp://ftp.cs.wisc.edu/pub/users/keles/849_TEX/lecture_102708.pdf) and article by [Smyth](http://www.statsci.org/smyth/pubs/EoB/bap064-.pdf).
