---
layout: post
title: "A Trick To Solve Recurrence Relation"
categories: algorithms recursion subtitution
---

_Not an extensive treatment of what recursion is/can-do. A simple trick I found interesting in solving recurrence relations._

## when our protagonist was a kid ...

So it started with the following ingenious/ingenuous proof. 

**Claim:** If $$ T(n) \leq T(\alpha n) + T(\beta n) + cn $$ where $$a + b < 1$$ then $$T(n) = O(n)$$ *

**Proof:** 
Suppose $$T(n) \leq kn$$.

$$ \implies T(n) \leq kan + kbn + cn $$ 

Now choose $$k$$ such that $$T(n) \leq kn $$ which holds for $$ k = \frac{c}{1-(a+b)} $$.

Well, the claim is if a recursive solution reduces the problem into a problem of size smaller by any constant factor less than itself, the number of subproblems does not increase the asymptotic runtime. _(This is because the whole recurrence formula translates into a geometric series anyway. Here, the ratio would be less than 1. So the first term dominates the sum.)_

## ... she wondered ...

But where did that $$ k = \frac{c}{1-(a+b)} $$ come from? Let's check if that holds to begin with.

Suppose $$T(n) \leq \frac{c}{1-(a+b)}n \implies
T(n) \leq \frac{c}{1-(a+b)}\alpha n + \frac{c}{1-(a+b)}\beta n + cn = kn$$

Great! It works! Then again, how? The issue became more pronounced because I had to solve the following recurrence relation:

Suppose $$a > b > 1$$ and $$c > 0$$ are constants and 

$$T(n) \leq aT \Big(  \frac{n}{b} \Big)  +cn; T(1) \leq c$$

Show that: $$ T(n) = O(n^{\log _{b} a}) $$

Finding that constant proved very problematic.

## ... but not too much ...
Turns out the trick is quite simple. We had 

$$T(n) \leq kan + kbn + cn$$ 

but we needed 

$$T(n) \leq kn$$

So let's just equate the two and solve for $$k$$: 
$$kan + kbn +cn = kn \implies k = \frac{c}{1-(a+b)}$$

## ... for her answers arrived soon ...
Now to solve the subsequent task, the process is the same. 

Assume $$T(n) \leq kn^{\log _{b} {a} } $$ which resolves the expression to $$T(n) \leq ak \frac{n^{\log _{b} {a} }} {b^{\log _b a}} + cn $$ 

Equate the two expressions and derive a neat value of $$k$$ that can be substituted to get a clean solution for the recurrence.

_tips: $$ {b^{\log _b a}} = {a^{\log _b b}}$$ (take $$\log$$ on both sides ) and $$a > b \implies 2b \leq (a + b) $$._

## ...after she lost the points.

For substition to work, the final expression has to be exactly the same as was assumed at the beginning. Anything extra at all doesn't prove the substitution was correct. This is all just to capture a clean form of the solution.

**Footnotes(?):** <br/>
\* If you are curious, the claim itself was used to analyze the runtime of task: given $$n$$ numbers, choose the $$k^{th}$$ smallest in linear time.
