---
layout: post
title: "SPOJ-DGAME"
date: 2013-10-18 21:16
comments: true
categories: [Editorial, SPOJ, Question of the Week, Game Theory, Game of Nim]
---

**Prerequisites** :  Game of Nim

**[Problem](http://www.spoj.com/problems/DGAME/)** : Game of Nim with variations. (N piles with i<sup>th</sup> pile containing either 2*i or 2*i+1 stones where even number of stones may be removed from selected pile.)

**Explanation** :

At every index i, can have stones in two ways, so total no. of ways comes out to be 2 * 2 * . . . . * 2 (n times).

Now whether an index have 2*i or 2*i+1 stones the game remains unaffected because if there are 2*i+1 stones in any pile there will be one stone left out anyways, and the game will be same as the one played with 2*i stones. We shall be considering only this case.

At any point of the game i<sup>th</sup> pile contains 2*(i - k) stones (where k is any arbitrary constant). Now you can interpret the question as the ith pile had i stones in the beginning and you can take out any positive number of stones from it. This is the standard nim game containing N piles, where the ith pile contains i stones.

Now our task is to find XOR of N numbers(1 to N). Every number can be represented 4*p+q where 0 <= q <= 3 and p is any arbitrary.

For every N of type 4*p+3, it can be easily proved that XOR of 1 to N is 0. So the first person will always lose the game. The answer for such numbers is zero.

For N of type 4*p+1 XOR of numbers from 1 to N is 1, so the first player can remove 1 stone from any pile containing odd number of stones so that the XOR becomes zero. There are (N+1)/2 piles containing odd number of stones, so the answer for this case is (N+1)/2.

For numbers of type 4*p or 4*p+2 XOR is N and N+1 respectively So first player can make XOR zero by removing stones from any pile whose most significant bit is same as the most significant bit of N. Thus the answer for this case is the number of piles which has x stones, such that when x is represented in binary, it has the same number of bits as the binary representation of N. This can be obtained by subtracting from N the maximum power of 2 which is less than or equal to N.

**Links** : [Game of Nim](http://community.topcoder.com/tc?module=Static&d1=tutorials&d2=algorithmGames)
