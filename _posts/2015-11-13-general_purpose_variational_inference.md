---
layout: post
category: inference
title: "General purpose variational inference"
---

In the [previous post]({% post_url 2015-11-13-elbo-in-5min %}) I derived the evidence lower bound (ELBO),
`\[\mathcal{F}(\lambda; x) = \int q(z;\lambda) \left[\log p(x,z) - \log q(z;\lambda)\right]dz ,\]`
which variational inference attempts to maximize. Now I'll describe a method to perform this maximization using only the model gradient $\nabla_z \log p(x, z)$. 

The approach I'll describe uses the so-called "reparameterization trick."[^1] First note that $\mathcal{F}$ is really just an expectation with respect to our approximating distribution $q$:
`\begin{align*}
\mathcal{F}(\lambda; x) &= E_{z\sim q}\left[ \log p(x,z) - \log q(z;\lambda) \right] \\
&= E_{z\sim q}\left[ \log p(x,z)\right] + H(q;\lambda)\end{align*}`
where I've made the simplifying assumption that the entropy $H(q; \lambda)$ is available in closed form (this is true for Gaussian approximating families, but if we're using some other weird family we can always move the entropy back into the Monte Carlo approximation). The expectation over $\log p(x, z)$ might not have a closed form, but we can approximate it by drawing $n$ samples $z_i \sim q(z;\lambda)$ and evaluating the empirical expectation
`\[
\hat{\mathcal{F}}(\lambda; x) = \frac{1}{n}\sum_{i=1}^n \log p(x,z_i) +  H(q; \lambda) .\]`
Our approach will be to do gradient ascent on this Monte Carlo approximation. But wait, you might object, $\lambda$ doesn't appear anywhere in (the Monte Carlo part of) this expression, so how can we compute a gradient? The answer is that $\lambda$ was a parameter of the distribution that produced $z$, so we just have to differentiate through the sampling algorithm, holding fixed the random seed  (this is the "reparameterization trick"). In many cases this is straightforward to do. 

For example, if $q$ is Gaussian parameterized by a mean and standard deviation $\lambda=(\mu,\sigma)$, the sampling procedure would first sample a standard Gaussian variable $\varepsilon \sim N(0, 1)$ and then compute the transform $z = \sigma \varepsilon + \mu$. Rewriting our Monte Carlo ELBO in terms of these "base variables" $\varepsilon_i$,
`\[\hat{\mathcal{F}}(\lambda; x) = \frac{1}{n}\sum_{i=1}^n \log p(x,\sigma \varepsilon_i + \mu) + H(q; \lambda) \]`
we can now easily differentiate this expression with respect to $\mu$ and $\sigma$ (by the chain rule, this will involve the gradient $\nabla_z \log p(x, z)$). The result is a stochastic estimate of the gradient of the ELBO, which you can plug into your favorite stochastic optimization algorithm (SGD, Adagrad, etc.). 

Note the only assumption we've made about the model is that we have access to gradients $\nabla_z \log p(x, z)$, which is nearly always the case thanks to automatic differentiation. This is how Stan implements variational inference for arbitrary models (more details in [their paper](http://arxiv.org/abs/1506.03431)), and many other languages now support autodiff as well. For example, using [autograd](https://github.com/HIPS/autograd) for Python the entire algorithm can be implemented in under 140 characters:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/DavidDuvenaud">@DavidDuvenaud</a>&#10;def elbo(p, lp, D, N):&#10; v=exp(p[D:])&#10; s=randn(N,D)*sqrt(v)+p[:D]&#10; return mvn.entropy(0, diag(v))+mean(lp(s))&#10;gf = grad(elbo)</p>&mdash; Ryan Adams (@ryan_p_adams) <a href="https://twitter.com/ryan_p_adams/status/663049108689715200">November 7, 2015</a></blockquote>

If model gradients are not available, it's still possible to estimate the ELBO gradient using a trick from reinforcement learning, described in the paper [Black Box Variational Inference](http://arxiv.org/abs/1401.0118). However, this estimate is higher-variance, so optimization will converge much more slowly than when model gradients are available. 

[^1]: This trick was introduced by [Kingma, Salimans, and Welling](http://arxiv.org/abs/1506.02557) in the context of variational autoencoders, and explored in more depth in a [nice post by Shakir Mohamed](http://blog.shakirm.com/2015/10/machine-learning-trick-of-the-day-4-reparameterisation-tricks/).

<script src="//platform.twitter.com/widgets.js" charset="utf-8" >
</script >
