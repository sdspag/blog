---
layout: post
title: "SPOJ-RRSCHED"
date: 2013-10-03 18:38
comments: true
categories: [Editorial, SPOJ, Question of the Week, BIT, Binary Indexed Tree]
---
**Problem** : [RRSCHED](http://www.spoj.com/problems/RRSCHED/)

**Difficulty** : Medium

**Prerequisites** : BIT

**Explanation** : 

A very naive approach would be to iterate over time and keep on decreasing the no of tasks to be performed and storing the task completion time in a separate array.This implementation would definitely give TLE  O(N*T) with the given constraints.
	
In the above approach we can observe that we can do better by jumping times from t = completion of easiest task (w.r.t time) to t = completion of next easiest task . 

For this we need to sort the input array w.r.t time .But we would also want their original indices(because order matters).So you can use pair<int,int>  to store both the index and time  of a task.That way you would not lose your index’s by sorting the array.

In the following discussion:

Pair [i] denotes i th pair in sorted array
Pair[i].Time denotes time needed for ‘Pir[i].Index’ th task to complete.
 
Now for every iteration all we need to do is to keep track of  Current time and do as follows.

1. Time Taken for present task to be completed
	= 	(no of remaining elements)\*(Pair[i].Time-Pair[i-1].Time) +
 		1\*no of Tasks before Pair[i].Index which are not completed

2. Extra Time to complete the  round
	= 1\*no of Tasks after Pair[i].Index which are not completed.

3. Update Pair[i].Index as completed

Thus BIT comes in handy here where we have to query the no of tasks not completed and update a task as completed in O(logN). For this purpose initially keep a BIT array and update every element by ‘+1’ which means that the task is not completed yet.You can use Update(i,-1) to update the task as completed. You can find the no of tasks which are not completed  before an index i by Query(i).

The [pesudo code](http://code.hackerearth.com/d8db76J) is as follows:


	//Initialize bit array to zero 
	
	BIT[MaxN] = 0

	// Update bit array 
	
	Upadte(i , x)
    	for(; i <= n; i += i&-i)
        	BIT[i] += x
	
	//Query Cummulative Frequency in a bit array 

	Query(i)
    	s=0
    	for(; i>0 ; i-=i&-i)
        	s += BIT[i]
    	return s

	//Take Input and Do the necessary initializations for BIT array
	
	TakeInput()
	for(i = 1; i <= N; i++)
    	Update(i, 1)
    
	//Main Part
	
	CurrentTime = 0
	for(i = 1; i <= N; i++)
    
    	//N-i+1 total uncompleted tasks
    	
    	CurrentTime += (Pair[i].Time - Pair[i-1].Time - 1)*(N-i+1)
    	
    	//query for uncompleted tasks before the present task
    	
    	CurrentTime +=  Query(Pair[i].Index)
    	
    	//Store the final anwer for the present task
    	
    	Answer[Pair[i].Index] = CurrentTime
    	
    	//Update current task as completed
    	
    	Update(Pair[i].Index, -1)
    	
    	//Increase by no of uncompleted tasks after the present task
    	
    	CurrentTime += (N-i) - Query(Pair[i].Index)

	//Note that the above psudo code also handles the case where two tasks have equal completion Time.
 

