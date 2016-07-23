---
layout: post
title: "Codeblitz : Editorial"
date: 2016-07-24 00:15
comments: true
categories: [Editorial]
---
After a fierce competition in the 3 hour long [Codeblitz](https://codevillage.sdslabs.co/competitions/CBLITZ16), Saurabh Goyal finished first with 5 problems in his pocket. One problem, Tricky Queries remained unsolved.


This blog post contains the editorials of the competition : 

#Tricky Queries

Prerequisites: A good understanding of Square-root Decomposition, and [Mo’s Algorithm](https://blog.anudeep2011.com/mos-algorithm/)

This problem can be solved by a modified version of square-root decomposition, mainly used in problems where Mo’s can be applied, but updates prevent us to do so.

Break the array into blocks of size n^(⅔) , so you have n^(⅓) blocks.
Consider a subarray starting at the start of block x and ending at the end of block y. Lets call such a subarray “special”. We have O(n^(⅔)) special subarrays. We can precompute the answer for all special subarrays such that 0 <= x <= y < n^(⅓). This takes O(n^(5/3)) time.

For each update occuring at pos, update the precomputed answer for all special subarray's in which pos lies. We can update in O(1) time for a subarray, this will take O(n^(⅔)) time per update.

For query L,R: find the largest special subarray which lies completely inside L,R , and update answer for all remaining elements, which will be O(n^(⅔)) in the worst case.
All operations have become similar to Mo's algorithm's "add" and "delete" operations. We have O(n^(5/3)) precomputation, and O(n^(⅔)) per query and update. Hence, time complexity: O(n^(5/3) + q*n^(⅔))


#Easy Queries


Since the values of the elements in the array is less than 100000, we can maitain a count
array that stores the count of each element.

If we apply seive over this array, we will be able to create another array, lets say dp, which
will store the number of elements in the array whose multiple is i. The answer initially would be
summation(dp[i] * count[i]).

In the first query, we just need to print the ans. ­> O(1).

In the second query, we need to update all the elements which have a[IND] as their
multiples, decrease the count of a[IND] by 1 and increase the count of VAL by 1. We also
need to decrease the number of multiples of a[i] and add the multiples of VAL to ans. Since
the number of factors of VAL is O(sqrt(VAL)) and since the maximum value of VAL is
100000, the update operation will take at max sqrt(100000) iterations. Complexity : O(sqrt(MAX_VAL) * Q) where MAX_VAL = 100000


#Lamps

Given is a m*n grid. If m>n then swap(n,m).

We need to place minimum number of lamps to lit the entire grid. 
If we place lamps in each row(as this dimension is smallest) at different columns we will lit the entire grid.

Rest remain is choosing m columns from n given columns. We can also permute them.

Hence answer = choose(n,m) * m!

#Subset Sum

In this question we have to find the path in graph such that sum of values 
of nodes in path is maximized . As we can see, we can always select all nodes which belong to the same scc, hence we can reduce every [Strongly Connected Component](https://en.wikipedia.org/wiki/Strongly_connected_component) to a node and form another graph which is directed acyclic graph(DAG) .
Now the problem is reduced to finding largest sum path in a DAG ! We can simply do this using dp on tree.

Initialize dp[i] with sum of values of all node of scc i.
Now, topologically sort the graph [link](http://www.geeksforgeeks.org/topological-sorting/), traverse the nodes in topological order and calculate dp[i] for all components.
dp[i]=dp[i] +max(dp[j]) where j is adjacent to i;
maximum over all values of dp is our answer .

#Bracket Sequence

To get the maximum length of a correct bracket sequence we have to remove the minimum number of brackets from the sequence which are not balanced.
Keep a count of opening ‘(‘ and closing ‘)’ brackets and the brackets to be removed. 
Iterate through the string for each opening bracket increase its count by one and for each closing bracket decrease the count of opening bracket by one if it is >=0 else remove that closing bracket .
After one iteration remove the number of opening brackets left (not balanced).
Maximum length of a correct bracket sequence is length of string minus no of brackets removed.

Complexity : O(n) where n is the length of the string.

#Distinct Sum

Let prev[x] stores the last occurence of x.
Initially prev[x] = 0 for all x.

Pseudocode :

	Long long ans=0;
	for(int i=1;i<=n;i++) {
		// try to find how many subarray contains this element
		Int p=prev[a[i]]+1;	// finding the left most index
		// right most index will be n itself
		Long long partof = 1LL * (i-p+1) * (n-i+1);
		ans+=partof * a[i];
		// update the prev array now
		prev[a[i]]=i;

	}

