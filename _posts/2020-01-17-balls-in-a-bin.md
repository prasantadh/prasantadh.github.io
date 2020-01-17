---
layout: post
title: "Balls in a Bin"
categories: counting probability
---
*Experiments with r balls and n bins to learn the basics of counting. Adapted from notes to Discrete Mathematics lecture at NYUAD*

## I 
**Q:** Given `r` numbered balls and `n >= r` bins, each bin allowed to take more than 1 ball, how many ways are there to do this?

**A:** Pick a bin for a ball. For each ball, there are `n` choices. For `r` balls, there are `n^r`choices.

*Remarks:*
- The argument works for every `r <= n` or `r > n`.
- The balls and the bins each have distinct identity. We can refer to ball `i` and bin `j`.

## II
**Q:** (Same as **I**) but now a bin is not allowed to take more than 1 ball?

**A:** 

The first ball has `n = (n-1+1)` choices. 

The second ball has `(n-1) = (n-2+1)` choices. 

... 

The `r`th ball has `(n-r+1)` choices.

Total choices = `n(n-1)(n-2)...(n-r+1)`

## III 

**Q:** (Same as **II**) but now all balls are identical.

**A:** Notice that the answer for **II** is $$\frac{n!}{(n-r)!}$$. Now, additionally all the `r` bins that have the balls are also identical. So total ways $$= \frac{n!}{(n-r)!r!}  =  {n \choose r} $$

*i.e.* This is equivalent to number of ways of choosing `r` bins where the balls go.

Total ways: `nCr` 

## IV
**Q:** (Same as **II**) but $$q_1$$ balls have same number.

**A:** Instead to `r` identical ball, we now have only $$q_1$$ identical balls. So with similar reasoning as **III**, total number of ways $$ = \frac{n!}{q_1!(n-r)!} $$

Note: If another $$q_2$$ balls had the same number, the total number of ways would be $$ \frac{n!}{q_1!q_2!(n-r)!}$$

## V
**Q:** Twist: All `r` balls identical. All bins distinct. But a bin is ALLOWED more than one ball.

For this we line up `n-1+r` positions: `r` positions for balls and `n-1` positions for delimiters \|. Now we need to decide which ones become balls and which ones become delimiters. Example: three balls and three bins: `---||`

Total ways:<br/>
= number of ways to choose delimiters (rest become balls) = $${n+r-1} \choose {n-1}$$ 

= number of ways to choose balls (rest become delimiters) = $${n+r-1} \choose {r}$$

> Introduction to Probability [Blitzstein, Hwang] introduces this as Bose-Einstein in Chapter 1.

## VI
**Q:** `r` identical balls and `n <= r` bins. Each bin takes at least one ball.

*Left as an exercise*

Equivalently, we have n types of objects and we want to select r objects, ensuring we select at least one from each type.




