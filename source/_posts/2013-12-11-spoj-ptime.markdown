---
layout: post
title: "SPOJ-PTIME"
date: 2013-12-11 19:17
comments: true
categories: [Editorial, SPOJ, Question of the Week, Factorials, Primes]
---

**[Problem](http://www.spoj.com/problems/PTIME/)**:  To write the prime factorisation of N! (N factorial).

**Explanation** :

The prime factorisation of N! would only contain primes less than  or equal to N.  Our main job is to find out the power of each prime factor and then write the prime factorisation in the correct form.

Since the given N is always greater than or equal to 2, its factorial (N!) would have 2 in its prime factorisation. Now let us start by finding the power of 2 in the prime factorisation of N!

N!  =  1\*2\*3…….(N-2)\*(N-1)\*N

The number of integers less than or equal to N having a factor of 2 is : Floor(N/2)

Some of the integers would be having more than one 2 in their prime factorisations ( 4 = 2\*2, 12 = 2\*2\*3)
So the number of integers less than or equal to N having a factor of 4 ( 2^2) is : Floor( N/(2^2) )

Similarly the process will continue till 2^i > N.

So the power of 2 in the prime factorisation of N! would be:
 Floor( N/2 ) + Floor( N/(2^2) ) + Floor( N/(2^3)  ) + …….. + Floor( N/(2^i) )                             
                                             where i is the highest number with 2^i <= N

Similarly doing the process for all the prime factors  less than or equal to N, prime factorisation of N! can be obtained where N can be as large as 10000. 
 
 


*Pseudocode* :

```
for i = 2 to N
	k = 0
	for a = 2 to sqrt(i)
		if i % a = 0
			k = 1
			exit loop
	if k = 0           // i is prime
		s = 0
		e = N/i
		while e > 0
			s = s + e
			e = e/i
		// after loop completes s would be the power of i in prime factorisation of N!	
```