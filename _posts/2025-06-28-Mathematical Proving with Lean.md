---
author: "Daniel Sherwood"
layout: post
title: "Learning to Express Mathematical Proofs with Lean"
date: 2025-06-28 19:09:00 -0500
lead: "Computer Literate...proofs"
---

**A decade ago**, when I was learning ```R```, I stumbled upon the ```.Rmarkdown``` file type and its use in (what was then) `RStudio`. 
I was amazed! I could blend code with english, intertwining examples with analyses. 
Well, it's a decade later and I no longer use ```R```.  However, I have since become interested in mathematical proving, and so I wanted to share with you a tool that does the *opposite* of ```.Rmarkdown```. 
It takes human "readable" mathematical proofs and turns it into code.
The language is called ```lean```. It's main utility is allowing for the *automatic* verification of proofs. 

Before I demonstrate with an example, please visit [The Natural Number Game](https://adam.math.hhu.de/#/g/leanprover-community/nng4)
for yourself and give it a try! 

What follows is a spoiler for the "tutorial" world. 

## Tutorial World Boss
The "boss" level challenge is to prove ```2 + 2 = 4```. I found this quite challenging! 
Here is my solution: 

```{lean}
nth_rewrite 2 [two_eq_succ_one]     //2 + successor of 1 = 4
rw [add_succ]                       //successor (2 + 1) = 4
rw [four_eq_succ_three]             //sucessor (2 + 1) = succesor 3
rw [three_eq_succ_two]              //successor (2 + 1) = successor ( successor 2 )
rw [two_eq_succ_one]                //successor ( successor 1 + 1 ) = successor ( successor ( successor 1 ) ) 
rw [succ_eq_add_one]                //successor 1 + 1 + 1 = successor ( successor ( successor 1 ) ) 
rw [succ_eq_add_one]                //1 + 1 + 1 + 1 = sucessor ( successor ( 1 + 1 ) ) 
nth_rewrite 2 [succ_eq_add_one]     //1 + 1 + 1 + 1 = sucessor ( 1 + 1 + 1 )
nth_rewrite 1 [succ_eq_add_one]     //1 + 1 + 1 + 1 = 1 + 1 + 1 + 1
rfl                                 //check reflexive property 
```
This solution is, well, not very inspired. Surely there is a better way, right? 
Well, the actual solution turns out to be just this: 

```{lean}
nth_rewrite 2 [two_eq_succ_one]     //2 + succ 1 = 4 
rw [add_succ]                       //succ (2 +1) = 4
rw [one_eq_succ_zero]               //succ (2 + succ 0) = 4
rw [add_succ, add_zero]             //succ ( succ 2 ) = 4
rw [← three_eq_succ_two]            //succ 3 = 4
rw [← four_eq_succ_three]           //4 = 4
rfl
```

Essentially, instead of breaking things down into **1**'s and adding, a more correct way is to express the **2**'s as **successors** and then add.

Definitely more elegant. 
Looking a little more closely at their solution I'm realizing that I didn't realize I could run several statements together. 
Also, that little arrow seems to indicate a reversal of the definition. 
Like, if ```h: X = Y``` then the arrow does ```Y = X```, I think. 

Even though I have spoiled the "first" boss for you, I hope you still take some time and play with it yourself. 
