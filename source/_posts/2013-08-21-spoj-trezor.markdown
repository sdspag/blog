---
layout: post
title: "SPOJ-TREZOR"
date: 2013-08-21 00:29
comments: true
categories: [Editorial, SPOJ, Question of the Week]
---
[Problem](http://www.spoj.com/problems/TREZOR/) :

Given a 2-D Coordinate Plane, find how many points are visible from one point. Two persons are standing at two other points, and you need to find how many other points are visible to both, points visible to only one, and none.


Explanation:

For Simplicity consider one guard standing at **(0,0)** and rectangle of size **L\*(A+B+1)** is present with corner points **(1,0) (L,0) (1,A+B) (L,A+B)** and other guard is standing at **(0,A+B)**.
    
Lets discuss the solution to one guard. A point **(X,Y)** will be blocked by another point **(X1,Y1)** to the origin if the slopes of lines joining points are equal, i.e **X / X1 = Y / Y1 = Z (Z > 1)**. That means **X = X1 \* Z** and **Y = Y1 \* Z**, which inturn means that **X** and **Y** are not coprimes (have a common factor). So for every horizontal row **(i,Y)** number of points visible to origin are number of **i’s** from **1 to L** that are coprime to **Y**.
     
Similar is the situation to other guard, points of the form **(i,Y)** are visible if and only if **‘i’** is coprime to **(A+B-Y)**. and visible to both if **‘i’** is coprime to **(A+B-Y)** and **Y**, which means **‘i’** is coprime to **(A+B-Y)\*Y**..

Now the problem is reduced for a given **Y** , how many numbers from **1 to L** are coprime to **Y**. For this we can prime factorize **Y** and use inclusion and exclusion principle on primes to find the final answer. Total there can be at max 6 prime factors to **Y**, as the product of first six primes  > 4000 = **(A+B)**. Now it takes **2^N = (2^6) = 64** steps for finding number of coprimes of each Y, so 3 * 64 for each horizontal row, so (3 * 64 * 4000) for all rows  = 768000 steps which comes under time limit.