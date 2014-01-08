---
layout: post
title: "SPOJ-GEEKOUNT"
date: 2014-01-08 15:54
comments: true
categories: [Editorial, SPOJ, Question of the Week, Ad-Hoc]

---
[Problem](www.spoj.com/problems/GEEKOUNT): Let f(x) be the product of digits of a number. Given L and R, find the number of values of 'i' such that L <= i <= R and f(i) is even.  

Pre-Requisites : None

Explanation :

We have to find the count of such numbers, between 2 given numbers, whose product of digits is even. Any number, in which any one digit is even, will come in this category. We will find all those numbers whose product of digits is odd and subtract them from total numbers between two given numbers. We will calculate required numbers from 1 to first number and lets call it num1, then from 1 to second number and lets call it num2 , then num2-num1 will be the answer.

Suppose we consider any n digit number. Then the number of numbers with exactly n digits and all digits odd will be 5 raised to power n. So, the number of numbers from 1 digit to n digits with all digits odd will be sum of all powers of 5 raised to power 1 upto n 
ie. 5^1 + 5^2 + 5^3 +.......+ 5^n.

The above formula of 5 gives the ans as 999..9(n times). But what about the case when the number is neither the smallest, nor the largest n digit number.

So if we are a general n digit number, then we will use formula to calculate the required number of numbers upto n-1 digits and use the following approach to calculate the number of n digits numbers, smaller than given number, for which product of digits is odd.

The number of such numbers will be equal to sum of the numbers of product of half of corresponding digit and 5^ [length-position-1] and  this digit will be from left digit to right digit. However, this process will stop when we encounter an even digit because from there we will not get any number where all digits will be odd.

Pseudocode : 
    
    sum[0] = power[0] = 0
    for i = 1 to 10 :
	    power[i] = power[i-1] * 5 
	    sum[i] = sum[i-1] + power[i]
    //dig1 is string form of first number.
	//dig2 is string form of second number.
	//n1 stores the number of numbers whose product of digits is odd.
	//length1 is length of first string.
	//length2 is length of second string.
	n1 = sum[length1-1] 
	//Calculating upto length1-1 digits
	for j = 0 to length1:
	    n1 = n1 + dig1[j]/2 * power[length1-j-1]
	// n1 gives the number of numbers with all digits odd from 1 to dig1.
	



Do the same for second number.
	

Subtract n1 from n2 and subtract this result from the difference between the 
given 2 numbers to obtain the required answer.


Let us take an example. Suppose we have to calculate till 57981. So, from 10000 to 50000, our counting will be only from 10000 to 19999 and 30000 to 39999, so and the number will be equal to same which we found for 4-digit numbers. Then we will shift our counter from 5 to 7 in our number 57981. Now, our target will be from 1000 to 1999, 3000 to 3999, 5000 to 5999, and our answer will be same as we found for 3 digit numbers. Process terminates as we encounter 8 as after it, no more numbers with all digits odd.