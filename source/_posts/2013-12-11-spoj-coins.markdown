---
layout: post
title: "SPOJ-COINS"
date: 2013-12-11 19:39
comments: true
categories: [Editorial, SPOJ, Question of the Week]
---

**[Problem](http://www.spoj.com/problems/COINS/)**: Given, a gold coin in Byteland, find the maximum amount of American dollars you can get for it. 

**EXPLANATION** :

Consider a golden coin with a number ‘n’ on it. Now, the coin can be exchanged by n/2, n/3, n/4 coins only (all rounded down). Obviously, one would exchange the coins only if n/2+n/3+n/4 is greater than n. And similar holds for n/2, n/3, and n/4 and so on. Looking into the pattern carefully, all the numbers less than 12, if exchanged with coins, would return either lesser or equal to the number. So, it is better to exchange them directly by American dollars (1:1). Let's try to solve it with a recursive function (a function in which we call our same function to solve it) :

*Pseudocode of the function* :

```
Return type coin (variable type n)
if(n<12)
	return n;
else
	return summation over i of (maximum of n/i and coin(n/i))
	//here i is 2,3 and 4.
```

Ok, now an example, say 40:
If I would exchange 40 by coins:-40/2+40/3+40/4=20+13+10=43. But that's not the correct answer.
20=20/2+20/3+20/4=10+6+5=21
Now, as all the numbers have been reduced to numbers less than 12, they would return the same value.
13=13/2+13/3+13/4=6+4+3=13
10 being lesser than 12 would return the same value. So, 21+13+10= (44) American dollars would be the maximum one can get.

Considering 81:
81/2+81/3+81/4 = 40+27+20
40=20+13+10;
27=13+9+6=28;
20=10+6+5=21;
Correct answer being 93.
Notice that 20 is being tested again (resulting from 40). We thus try to solve this problem with memoization (technique of storing already-calculated values for a function in a cache) which optimizes the solution.
It stores the value once calculated in the function and returns this value when called without performing those calculations again. So, the pseudo code of the function finally becomes:

```
One must use the map library for using memoization.

static map<variable(n) type, function return type> memo;
if(n<12)
	{return n;}
if(memo.count(n)>0)
	return memo[n];
else
	long long int ret;
	ret= summation over i of (maximum of n/i and coin(n/i));  i=[2,3,4]
	memo[n]=ret;
	return ret;
```

This makes the function well within the time-limit.