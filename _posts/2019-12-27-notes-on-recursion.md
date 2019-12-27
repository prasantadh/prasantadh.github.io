---
layout: post
title: "How to eat an elephant: Casual Notes on Recursion"
categories: algorithms recursion
---

_Disclaimer: No elephants were harmed in the making of this post._

Well, the adage goes "One bite at a time." Absolutely true. No other way, really. But for the purposes of our discussion, we ask a slightly modified question:

## When have you eaten an elephant?
You gotta know when the deed is done, right? You gotta know when to stop.

So. When do you stop? 

_"When you have eaten the last bite."_ There we go. That's it. That's when you know you have eaten the elephant. That's when you stop.

But then, what has this got anything to do with recursion?

And I say, This captures all key notions of recursion.

### 1. To eat the last bite...
...you need to have ALREADY eaten the second to last bite

And to eat the second to last bite, you need to have ALREADY eaten the third to last bite.<br/>
And to eat the third to last bite, you need to have ALREADY eaten the forth to last bite.<br/>
...and so on...<br/>
...and so on...<br/>
...you get the idea...<br/>
...

This encodes the idea of __dependency/progress__. 
We solve a bigger problem by solving a smaller problem.
_One bite at a time._

The smaller problem has to be chosen with some care though. Eating second to last bite of KitKat doesn't help eat the last bite of elephant at all.

### 2. And when you get to the first bite...
Well, you just eat it. There is no dependency on the first bite. 

This is what is called the **base case**. 
Terminology aside, there is only so much we can split a problem into. 
At some point, the problem is small enough and we eat it head on. Then we do the next thing. _One bite at a time._

### Oh! Oh! Is this top-down?
Yes, it is. That's what it is called. **Top-down** approach. We took a zoomed out picture of the whole elephant from afar and each time we worked to see less and less of the elephant until all we could see was one bite. The first bite.

### Is there an opposite of top-down?
Yes. It's called **Bottom-Up** approach. We know the dependency. We know there is nothing to do unless we take the first bite. So we take the first bite. This allows us to take the second bite. Then the third. Then the forth......Then the last.

Is this recursion? No. But for every top-down recursion out there, is there a bottom-up approach? Yes.

