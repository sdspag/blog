---
layout: post
title: "SPOJ DQUERY"
date: 2013-07-29 15:40
comments: true
categories: [Editorial, SPOJ, Question of the Week, Binary Indexed Trees, Sorting, Offline Programming]
---
**[Problem](http://www.spoj.com/problems/DQUERY/)** :
Given a list of integers, and you need to answer *Q* queries, in each of which given two indices *i* and *j* and you need to answer how many distinct integers are present whose index lies between *i* and *j*.

**Pre-requisites** : Binary Indexed Trees (For Range Sum Queries), Sorting, Offline Programming

**Explanation** :
Let us try to understand the solution for a simpler problem where *i* = 1 for all queries. Now for every integer if we just mark the first position in the list, for every query the answer will be just the number of integers marked among the first *j* places.

If we use a boolean array to store the marked integers and run a loop for every query through all the starting j numbers the complexity of the algorithm will be O(N) for each query.

But if we use some advanced data-structure like Binary Indexed Trees, we can acheive this in O(logN) time for every query.
So our final complexity for this simpler problem is O(QlogN).

For our main problem, we need to know a style of programming called *Offline Programming*.

There are two types of such classification, 
1) Online programming
2) Offline Programming

In online programming we answer each query as soon as we take it as input.

In offline programming we take all the input at once and try to make some changes to the order of input and solve all queries and then print the output for each input in the original order.

For this question we will need to use Offline Programming, where we take all the queries at once and sort them in the ascending order of the left indices *i*.

Now starting from left i.e, i = 1, we already had the solution of O(Qlog(N)) given above and we will be moving right one by one. Now for solution to all the queries with left index greater than *i* we just have to ignore the integer at index *i*. So we will unmark the integer at index *i*, and mark the left most index to the right of index *i* whose value is equal to arr[i]. Then we repeat this for index *i+1*.

This marking and unmarking of the indexes can be acheived in O(log(N)) time using binary indexed trees.

We need the left most index of the integer at index *i* to mark in the above solution, to acheive this for every index we can check all the integers to the right and store the left most value , but rather we can move from right to left and store present left most index of every integer at every step.(See pseudocode for this) and maintain an array for left most index to the right.

After we get ouput for every input, we will rearrange the input as the original order.

**Pseudocode** :

arr[] is the input array

BIT[] stores the marked array.

BIT[i] stores 1 if element is marked or 0 if it is not marked

LMITR[i] stores Left Most Index To Right such that arr[ LMITR[i] ] == arr[i]

PI[] stores the present index of an integer.

```
Take input

loop for i = 1 to 1000000 
    PI[i] = INF
end loop

loop for i = n to 1
	LMITR[i] = PI[ arr[i] ]
	PI[ arr[i] ] = i
end loop

loop for i = 1 to n
	loop for all queries starting with i
		find the sum of first j elements of BIT and store it.
	end loop
	ummark index i
	mark LMITR[i]
end loop

print output
```