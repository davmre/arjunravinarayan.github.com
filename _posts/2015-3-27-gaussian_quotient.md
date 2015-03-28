---
layout: post
category: statistics
title: "Quotients of Gaussian Densities"
---

*tl;dr: I was confused about the precise expression for
  the quotient of two multivariate Gaussian densities, so I'm writing
 it up here.*

Suppose you want to multiply two Gaussian densities, `$N(x; a,
A)$` and `$N(x; b, B)$`.[^1] It's a
standard result[^2] that the product of two Gaussian densities is
an (unnormalized) Gaussian in the same variable,
`\[N(x; a, A)N(x; b, B) = \alpha \cdot N(x; c, C)\\
C = \left(A^{-1} + B^{-1}\right)^{-1}\\
c = C\left(A^{-1}a + B^{-1}b\right)\\
\alpha = N(a; b, A+B).\]`The precision matrices add, the means are averaged weighted
by precision, and interestingly the normalizing constant `$\alpha$` is *also* in
the form of a Gaussian density. All of this is straightforward, though annoying, to prove, just by
expanding out the product of the two densities,
[completing the square](https://learnbayes.org/index.php?option=com_content&view=article&id=77:completesquare&catid=83&Itemid=479&showall=&limitstart=1),
and collecting the terms that involve `$x$` (for the product), and those
that don't (for the normalizing constant). 

What happens if you take the *quotient* of two Gaussian densities?
This comes up, for example, in
[expectation propagation](http://research.microsoft.com/en-us/um/people/minka/papers/ep/roadmap.html),
where a newly-updated Gaussian approximate posterior is divided by the previous approximation
to get the "message" that would have transformed the latter into the former. It turns out the result is
`\[\frac{N(x; a, A)}{N(x; b, B)} = \beta \cdot N(x; d, D)\\ D = \left(A^{-1} - B^{-1}\right)^{-1}\\ d = D\left(A^{-1}a - B^{-1}b\right)\\ \beta = \frac{|B|}{|B-A|}\frac{1}{N(a; b, B-A)}.\]`
Note that the form of the message---the mean and covariance
`$(d, D)$`---is the same as you would have gotten by plugging in a
*negative* covariance matrix `$-B$` to the product formula above. This
follows directly from the standard exponential identity `$1/e^x = e^{-x}$`. A
Gaussian with negative-definite covariance is kind of a weird
beast: it's essentially a bell curve opening upwards instead of
downward, so it can't be normalized and is not a valid probability density, but
we can treat it as a formal object that "cancels out" a certain amount
of observation. If I have a Gaussian belief about some quantity, and
then observe that quantity with negative-variance Gaussian noise, I am
now *more* uncertain than I was before!

But now we get to the point of this post: the interpretation of
division as multiplication by a negative-variance density is workable
if you only care about the *form* of the result, but it falls apart
when you need to compute the normalization constant (as Bayesian
model evidence, for example). Plugging `$-B$` into the
formula for `$\alpha$` above does *not* give the correct normalization
constant for the quotient case; in fact it doesn't in general give a real
number! (the determinant `$|A-B|$` will be negative, so its square root
is imaginary). Doing the derivation from scratch for the quotient case
yields the correct normalization constant `$\beta$`, given above.

[^1]: Notation: let `$N(x; a, A) = \frac{1}{(2\pi)^{d/2}|A|^{1/2}}\exp\left(-\frac{1}{2}\left(x-a\right)^TA^{-1}\left(x-a\right)\right)$`
denote a multivariate Gaussian density in the variable `$x$` with mean `$a$` and covariance matrix `$A$`.
[^2]: I first saw this in the late Sam Roweis' [notes on Gaussian identities](http://www.cs.nyu.edu/~roweis/notes/gaussid.pdf).
