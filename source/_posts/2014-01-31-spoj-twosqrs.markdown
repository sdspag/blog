---
layout: post
title: "SPOJ-TWOSQRS"
date: 2014-01-31 17:02
comments: true
categories: [Editorial, SPOJ, Question of the Week, Ad-Hoc, Number Theory]
---
[Problem](http://www.spoj.com/problems/TWOSQRS/) : Given a number (N) and you need to find whether it can be represented as sum of two squares or not.

Pre-Requisites : None

Explanation :

Any number (lets say A) can be represented in form 4\*k+r  (where r can 0 , 1 , 2 , 3). Thus possible values for A^2 is 4\*k and 4\*k+1 , so sum of two squares can never be of the form  4\*k +3 .

N = p1 ^ x1 \* p2 ^ x2 \* ......  pm ^ xm (prime factorisation of N) can be represented as the sum of two squares if every prime of the form 4\*k +3 have even degree (or power) .
    
This can be proved using following 5 statements :-

1.  The product of two numbers, each of which is a sum of two squares, is itself a sum of	 	two squares.
2. 	If a number which is a sum of two squares is divisible by a prime which is a sum of two 	squares, then the quotient is a sum of two squares.
3.	 If a number which can be written as a sum of two squares is divisible by a number 	which is not a sum of two squares, then the quotient has a factor which is not a sum of 	two squares.
4. 	If a and b are relatively prime then every factor of a^2 + b^2 is a sum of two squares. 
5.	 Every prime of the form 4n+1 is a sum of two squares.	


For proof of these statements refer [here](http://en.wikipedia.org/wiki/Proofs_of_Fermat's_theorem_on_sums_of_two_squares).