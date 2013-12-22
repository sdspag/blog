---
layout: post
title: "SPOJ MBALL"
date: 2013-08-05 15:26
comments: true
categories: [Editorial, SPOJ, Question of the Week, Dynamic Programming, DP, Memoization]
---
**[Problem](http://www.spoj.com/problems/MBALL/)** :
Given the possible ways of scoring in a game, you neeed to find in how many ways can a given score be achieved.

**Pre-requisites** : [Dynamic Programming](http://www.topcoder.com/tc?d1=tutorials&d2=dynProg&module=Static)(read DP)

**Explanation** :

The problem is quite simple. The different ways of scoring can be as follows:

*   safety: 2 points
*   field goal: 3 points
*   touchdown: 6 points
*   one-point conversion try after touchdown: 7 points
*   two-point conversion try after touchdown: 8 points

We start by subtracting the smallest possible score ie 2 from the given score and reduce it to smaller instance of the same problem. This way we continue till we reach 0. Then carry out this step with 3,6,7and 8 as well.

eg 10 points can be scored in following ways:

*   first 2 points and then scoring 8 points.
    *   2 points can be scored in 1 way only and 8 points can be scored in following way:
        *   first 2 points and then 6 points.
        *   first 3 points and then 5 points.
        *   first 4 points and then 4 points.
*   first 3 points and then scoring 7 points.
*   first 4 points and then scoring 6 points.
*   first 5 points and then scoring 5 points.

In each case we reduce the problem to a subproblem of the same type.

Notice a pitfall here. Even if there is just one test case (say S=10 itself), We are doing the computation for various subproblems again and again. eg we calculate the number of ways of scoring 6 points when we are calculating the number of ways of scoring 8 points. And we are doing this calculation again when we try to find number of ways scoring 10 points. 

Basically we are solving the same sub problem again and again even when we have just one test case. Surely this method will timeeout. 

So what do we do? We use DP. DP means we save the result of various sub problems so that we do not have to calculate them again and again. In this case, we make an array called *result[N]* (and initialise with -1) in which we store the number of ways of achieving *i* score for all *i* varying from 2 to *N*. Also there is only one way of achieving score of '0'. So *result[0]*=1. Now when we need to calcualte the number of ways of achieving score *i*, we check the array. If it contains -1, we calculate the result and update the array entry else we use the value from the array.
