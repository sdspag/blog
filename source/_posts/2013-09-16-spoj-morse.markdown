---
layout: post
title: "SPOJ MORSE"
date: 2013-09-16 18:30
comments: true
categories: [Editorial, SPOJ, Question of the Week, Dynamic Programming, DP, Trie, Digital Tree]
---

**Prerequisites** : Dynamic Programming, Trie

**[Problem](http://www.spoj.com/problems/MORSE/)** : 

Given the Morse code for English alphabet, a list of words(dictionary) and a Morse sequence, the task is to compute the number of distinct phrases that can be obtained from the sequence using words from the dictionary.

**Explanation** :

In this problem, we are given N words in a dictionary and a Morse string containing ‘.’ and ‘-’ of length L. Now we are asked to find how many strings from the dictionary words when converted to their Morse part will exactly map to the given Morse sequence.

Say we have .-..--.-..-.-…- and the dictionary as a aa bc acm shuka etc., one naive way is to generate all possible strings using the given words of the dictionary and see if they map to the given Morse string or not. But this approach is too slow and can take centuries to calculate if number of letters in dictionary are large enough (O(L<sup>N</sup>)).

So, one thing to observe here is there will be several sub-cases counted a lot more times than required as in sub part of Morse string ‘.-..-’ might map to one english string and the rest part of Morse string doesn’t, so for calculating that this isn’t a valid string will itself take order O(L^N) time. 

Such problems where only number of solutions are to be found are approached as given:

1. Start iterating Morse string either from left or right.
2. At each point,check if the current string covered using the iterator maps to any word in the dictionary(So this iteration is finding the last(or first) word that could be placed from dictionary to the position of the iterator).
3. When a word is found,use recursion on the remaining string.

Here we were also given some constraints to help us out with our complexity analysis:

1. Length of Morse string : 10000
2. Length of a word : 20
3. Number of words in the dictionary : 10000 

The pseudocode of the above idea looks something like :

        Function TotalStrings(int i) //the current position of the iterator
		{

			for(k from i to Morselength) // Morselength stores the length of the Morse string
			{

				if string[i…k] is valid and in dictionary
				then answer = answer + TotalStrings(k)

			}
		   
			return answer;

		}

Disclaimer: The below arguments are assuming we can search a Morse in a dictionary in O(1) time.

The above solution still doesn’t seem to ease our problem as we still have to face the worst case time complexity as O(L^N) but whenever we find some repeating subproblems we should always think of Dynamic Programming.

Now the question is: What if in the above code, the value TotalStrings(k) was already known to us? In that case what would the time complexity be?

It will be linear if all values of TotalStrings(k) are pre-known. Here comes the concept of Dynamic programming in which we can store the values of TotalStrings(k) whenever we calculate it. As we use linear time for calculating all the TotalStrings(k) with k in (0,Morselength),we use a total of O(L^2) time where L is the length of the Morse string.

All we need to change in code is before returning answer, just store the answer calculated in some global array so that next time the same value is asked it could be returned immediately.

Now,taking advantage of the constraints :

Length of a word cannot exceed 20 as given in the question,hence the conclusions are :

1. Each alphabet can have maximum 4 ‘.’ or ’-‘ characters. So a word of length 20 can have maximum 80 Morse characters
2. Hence in the inner loop of the above pseudo code k varies from i to i+80
3. If the word searching is extremely fast, our complexity reduces to O(80*l)


The problem left now is word searching. We can think of various methods and techniques to find a map from dictionary to the iterated part being used.

1. Hash maps
2. STL library map
3. Brute force
4. Trie implementation

Of all the above methods,the most reliable for this problem is trie, given the constraints and the difficulty in using large length strings(~80).

Trie implementation is simple and can be performed using arrays as well as link lists.

**Links** :

[http://www.geeksforgeeks.org/trie-insert-and-search/](http://www.geeksforgeeks.org/trie-insert-and-search/)

[http://en.wikipedia.org/wiki/Trie](http://en.wikipedia.org/wiki/Trie)

**Related problem on trie** :

[SPOJ-REVFIB](http://www.spoj.com/problems/REVFIB/) (Difficulty Level: Hard)
