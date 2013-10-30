---
layout: post
title: "How Stock Exchanges Work"
category: 
tags: []
---

"The NYSE" looks, on the surface like a grocery store, except with moving prices.

This is certainly how it _appears_ to "retail investors" (schmucks
like you and me with a Vanguard or Schwab account). But that's an
abstraction layer on top of the _actual_ trading infrastructure.

It's called an exchange because it doesn't actually own the apples. It
allows apple sellers to come in and set up shop.

But it provides a service of _matching_ buyers to sellers. How does
the matching work? Through an order book.

What is an order book? The Bid book and the Ask book. Matches are made
based on a set of rules.

Rules are such that price matters, and if price is even, time matters.

This second tie-breaker is why HFT exists. Is this a good rule?
probably not. We could add more decimal digits, but...

1) would that be useful? Race to the time-bottom would turn into a
race to the pennies bottom. Hard to tell what will happen.

2) Entrenched players who are happy with current system.

The space of rules is big, but nobody really wants to mess with the
system.

The people who don't like the rules try to set up their own exchanges,
called "dark pools". This isn't ideal for the world (it is not
transparent). Say what you will about the implicit tenets of the order
book, at least it's a ethos.

But absolutely a fact that the rules of the exchange mean that certain
strategies are viable. We could eliminate whole hosts of strategies by
setting different rules, but no real theory clearly points in one
direction over the other, so status quo remains.


That explains HFT. But let's break down the abstraction layer created
by brokers. First, why do brokers exist? because that abstraction
layer is nice, even though it is a fiction. But it comes at a
cost. (this transition is awkward, think about it...). Here's one neat
article that summarizes the issues that can come up when your broker
is tasked with executing trades: [Matt Levine
article](http://www.bloomberg.com/news/2013-10-30/rbs-promises-not-to-trade-against-clients-too-much.html)


But bottom line, Exchange prices are much more "volatile". When you
walk into a grocery store and you see $1.99/lb on the apples, you have
an expectation of supply liquidity (the number of apples you can see
stocked up), and price stability (the price won't change by the time
you walk to the checkout aisle). These assumptions go away if you're a
naked trader on Wall Street. The "ticker price" is a fiction, and
comes at a price --- although there are people (stock brokers) willing
to create that fiction for a small fee.


Further ideas:

You can think of the current price as encoding a certain belief about
the future. In the case of stock prices, it is future discounted
cashflows. But if we set up a prediction market or betting market
(think Intrade) we could encode arbitrary propositions.

In this case, you can view the entire order books as encoding a
"probability distribution". Link to Rajiv Sethi's blog post
here. However, this is not complete since there are strategic
incentives to not "show" your hand, except at the last minute to
execute a trade. (Since people knowing there is "support" at a certain
price reveals something that you might not want them to know).

This is why "technical analysis" even exists. (This is conjecture:
think about this more and hedge appropriately, and make it clear that
it is conjecture!!!).


