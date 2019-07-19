---
layout: post
title:  "Deep Dive into Binary Search"
categories: algorithms binary-search
---

_Disclaimer: This is not an educational piece on binary search (though I've retained a rough introduction). I have been confused about its exact implementation details. The piece is more for training myself on the intricacies of binary search and building an intuition for correct code._

## <a name="solution"></a> the Solution
Here's the code for binary search fresh off [it's wikipedia page](https://en.wikipedia.org/wiki/Binary_search_algorithm):
```pseudocode
L := 0, R := n - 1
while L <= R:
    M := (L + R) / 2 # integer division
    if (A[M] == T) return M;
    if (A[M] < T) L := M + 1;
    else R := M - 1;
return unsuccessful
```


## the Issue
When it comes to binary search, I am never quite sure if `while L < R` or `while L <= R`. Is `L := m+1` or `L := m`? Is `R := m - 1` or `R := m`? Are these `+1`s and `-1`s optimizations or strictly necessary for the correctness of code. What choices to make and why? And hence, this.

## the Intro
The basic idea is simple. The most common analogy used is consulting a dictionary.

Roughly like this:
1. Open at a random page.
2. Depending on the alphabetical placement, you will know if the sought-after-word should be in the chunk of pages to the left or to the right or on the page itself.
3. Repeat the process on the appropriate chunk.

Similar procedure applies if you are grading a stack of papers, and want to find the paper for one specific person. Assuming the papers are sorted by, say name, poke at a random location; check if the paper has that name; else repeat the process on the stack towards the top or the bottom. This example has a nice visual explanation [in this HackerRank video](https://www.youtube.com/watch?v=P3YID7liBug).

## getting A Bit More Technical...
When it comes down to implementations, the above procedure needs a couple more considerations:
1. One may not always find what one seeks. What to report then?
2. Let's divide the problem in half every time rather than at a random location. _I don't really have a good defense for this, other than truly random numbers can be expensive to generate, and even then difficult to generate correctly, and here, might not actually help the runtime complexity of the problem._
3. It is assumed that the input is sorted.

## the Classical Use Case
What [the wikipedia page on binary search](https://en.wikipedia.org/wiki/Binary_search_algorithm) has, and what most references seem to base the example code on is the following scenario:

Given a sorted (ascending) array _**A**_ of _**n**_ elements indexed _**0**_ through _**n - 1**_, and a target value _**T**_, find the index of _**T**_ in _**A**_.


## my Classical Faulty Implementation
Whenever I have to implement the binary search, here's where I start:
```pseudocode
L := 0, R := n - 1
while L < R:
    M := (L + R) / 2 # integer division
    if (A[M] == T) return M;
    if (A[M] < T) L := M;
    else R := M;
return unsuccessful
```
What follows is quite a bit of debugging, head scratching and feeling incompetent. So what's good about this? 

_Pros:_ It is the simplest implementation my brain can grasp. Initial `L` and `R` touch the boundary elements, No `<=`s on conditions, no `+1`s and `-1`s on updates! 

_Cons:_ **It is incorrect.**

Consider a simple case where there is exactly one element. Result `unsuccessful`. Wrong! Can we return the element if it is the only one and fix this? NO!

Consider the case when _**T**_ is greater than values contained in _**A**_. Infinite loop! 

On that note, consider this more illuminating example:_**A = {2, 3}**_ and _**T = 3**_. Infinite loop! R stays at index 1, L stays at index 0, the answer isn't found and the problem isn't updated over iterations at all. The reason here is that `(L + R) / 2` favors `L`. So if it ever comes down to two elements, it is always the `L` that is selected and the problem stops getting smaller. [The proof of correctness of binary
search](http://www.cs.cornell.edu/courses/cs211/2006sp/Lectures/L06-Induction/binary_search.html) depends on two factors: 1. It works for a base case and 2. At every iteration, the problem size decreases so that the base case is eventually reached. This approach violates both.

In the past, I have had some success with `while (R - L > 1)` then returning `L` or `R` per question and with other similar patches after numerous trial and error. However, a patch at best and even then it's a patch that's not easily generalizable across contexts.

**what If we do L \<\= R then?**
```pseudocode
L := 0, R := n - 1
while L <= R:
    M := (L + R) / 2 # integer division
    if (A[M] == T) return M;
    if (A[M] < T) L := M;
    else R := M;
return unsuccessful
```
Well, the same problem persists. If `L` and `R` are next to each other and the answer is under `R`, the loop condition is true and the problem is never updated.

**what If we update `L := M + 1` and `R := M - 1` then?** 
```pseudocode
L := 0, R := n - 1
while L < R:
    M := (L + R) / 2 # integer division
    if (A[M] == T) return M;
    if (A[M] < T) L := M + 1;
    else R := M - 1;
return unsuccessful
```
The same problems persists. Fails for single element. And if the answer is under `R`, by the time L gets to `R`, the loop already terminates returning `unsuccessful`. Try any example with `size(A) = 7` and `T = A[2]`.

Fixing all these issues, we get to that [fresh off the wikipedia page solution](#solution). But there's more. It is helpful to know what to do when there are 

## duplicate Elements Elements
This has been something else that pops off often; there are duplicate elements in _**A**_ (`A = {1,2,3,3,3}, T = 3) and one is required to find the leftmost or the rightmost element.

### Leftmost Solution
Now intuitively, the tweaks to the above standard code are simple to achieve this effect. Keep `R` at the edge of the solution. The closest R can approach is either the solution itself. Then let `L` catch up to `R`. If it has caught up, the solution is at `L`. However, sometimes `R` can be right next to the solution, in which case the solution will still be at `L` because the integer division favors it.


```pseudocode
L:= 0, R:= n
while L < R:
    M := (L + R) / 2
    if (A[M] < T):
        L := M + 1;
    else:
        R := M
return L // return (A[L] == T) ? L : unsuccessful
```

### Rightmost Solution
The basic idea is the same as leftmost Solution. The only tweak is to place `L` instead of `R` on the solution (`A[M] == T`) situation and let `L` keep moving right until it overshoots. Once it has moved past the target value, the previous index should be the required value.

```pseudocode
L:= 0, R:= n
while L < R:
    M := (L + R) / 2
    if (A[M] <= T):
        L := M + 1;
    else:
        R := M
return L-1 // return (A[L-1] == T) ? L-1 : unsuccessful
```

_Note: Additional insights can be gained into the workings of these algorithms by considering the result when the target value is less than the values in **A** or greater than the values in **A**._

## a Case Study (leetcode#69)
Implement `int sqrt(int x)`.

Compute and return the square root of _x_, where _x_ is guaranteed to be a non-negative integer. 

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned. _If x = 4, the function should return 2. If x = 8, function should return 2. This is beause the square root of 8 is 2.8284..., and since the decimal part is truncated, 2 is returned._ 

### Does the regular binary search work?
```pseudocode
L := 0, R := x - 1
while L <= R:
    M := (L + R) / 2 # integer division
    if (x / M == M) return M;
    if (x / M > M) L := M + 1;
    else R := M - 1;
return unsuccessful
```
Consider `x = 6`. Then `{L, R, M} = {0, 5, 2} -> {3, 5, 4}`. The algorithm should have stopped after the first iteration. However, the algorithm overshot. This is because the algorithm stops increment `L` only after it has passed the nearest smallest integer value. It is in the nature of the solution that `L` will overshoot. 

_Note: On the paper version of this post, I had checked and confirmed the same flaw with the leftmost element finding version of binary search. The issue is the same. `L` is incremented to catch up to `R`. By the time `L` reaches `R`, the solution has already overshot if it is not the exact solution._

Now we know a version of binary search that by design overshoots and is brought back to the last valid value: finding the right-most element when the target value are duplicates.

The final `c++` code that works:

{% highlight c++ %}
class Solution {
public:
    int mySqrt(int x) {
        if (x == 0 || x == 1) return x;
        int l = 0, r = x;
        while(l < r) {
            int m = (l + r) >> 1;
            if (x / m >= m) l = m + 1;
            else r = m;
        }
        return l - 1;
    }
};
{% endhighlight %}

### Future Work
What are other valid versions of binary search. There must be ways to keep `L` at boundary of the solution and keep `R` closer to and on the solution. Though this might require ceiling of integer division and not the floor.

Competitive Programming 3 [Felix Halim, Steven Halim] has a set of practice problems that are selectively on Binary Search. Solving them will help refine understanding.
