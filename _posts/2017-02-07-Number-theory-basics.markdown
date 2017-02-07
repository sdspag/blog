---
layout: post
title: "Number Theory basics"
date: 2017-02-07 23:55
comments: true
categories: [Editorial, Number Theory] 
---

Mathematics is widely used in computer science research, as well as being heavily applied to graph algorithms and areas of computer vision.
Problems in competitive programming usually require a good knowledge of Mathematics, especially Number Theory, Geometry and Combinatorics. 
This article discusses the theory and practical application of some of the basic concepts of Number Theory. You don’t need any special knowledge of Mathematics to understand these concepts, only a knack for solving problems is enough.


<b>Modulo Arithmetic</b>
The modulo operator gives the remainder on dividing one number by another . E.g. 10%3 = 1, 74%7 = 4 etc.
Properties :
(a+b)%m = (a%m+b%m)%m
(a*b)%m = (a%m*b%m)%m
(a-b)%m = (a%m-b%m+m)%m

<b>Modular Exponentiation</b>
Consider calculating a^b%m. It can be easily calculated using the naïve approach

__code__<br>
long long exponentiation(long long a, long long b, long long m)
{
long long ans=1;
for( int i = 0;i < b; i++)
{
            	ans *= a;
            	ans %= m;
}
return ans;
}

 
This approach is very inefficient as it has an O(b) time complexity but we may even have to solve it for b<=10^18. In such a scenario, we can use the highly efficient recursive function :
 
code
long long exponentiation(long long a, long long b, long long m)
{
            	if (b==0) return 1;
            	long long temp = exponentiation(a,b/2,m);
            	temp = (temp*temp)%m;
            	if (b&1)
return (temp*(a%m))%m; // if b is odd a^b = a^(b/2)*a^(b/2)*a
            	return temp;
}

 
This approach is a major advancement from the naïve approach as it reduces the time complexity to O(log(b)).
 



<b>Prime Numbers </b>
A prime number is a positive integer having exactly two factors : 1 and itself. Numbers which are not prime are called composite numbers.

Checking if a number is prime :
Given a number N we have to determine whether it is prime or not. This can be easily determined by checking if the number is divisible by any number greater than 1 and less than N. 
The time complexity for this will be O(N) but we know that if d|N then (N/d) also divides N. So for any composite number there will be at least one divisor d such that 1<=d<=sqrt(N).
So we only need to check divisibility of N with numbers less than or equal to sqrt(N).
So the time complexity of primality test is reduced from O(N) to O(sqrt(N)).

__code__<br>
bool isPrime (int N)
{
        	for(int i=2;i*i<=N;i++)
{
        		if(N%i==0)
return false; // if any number divides N then N is not prime
}
return true;
} 


Problem : <a href="http://www.spoj.com/problems/PRIME1">PRIME1</a>

Finding all prime numbers less than N : Sieve of Eratosthenes
To find all primes less than N, we can check the primality of every number but that would have a time complexity of O(N^1.5). Moreover, it would be redundant because we know that if a number is prime none of its multiples can be prime as it would be one of the divisors.
This fact is used in the famous prime generating algorithm called the Sieve of Eratosthenes :

__code__<br>
for( i=2 ; i<=N ; i++)
prime[i]=true; // Mark all numbers as prime
for ( i=2; i*i<=N;i++)
{
	if(prime[i]==true)  //if i is prime
	{
		for( j= i*i ; j<=N; j+=i )
prime[j] =false; // Mark multiples of j as not prime
	}
}

The time complexity of this sieve algorithm is O(NloglogN) which can be proved by using bounds for harmonic sums. 

SMPF (Smallest Prime Factor)
When we require to factorize small numbers (<=10^7) many number of times, then we can factorize a number in O(logN) time complexity after doing a sieve precomputation of O(NloglogN).
We can do so by doing a sieve such that arr[n] stores the smallest prime factor of the number n.

__code__<br>
for(i=1;i<=N;i++) arr[i]=i;
for(i=2;i*i<=N;i++)if(arr[i]==i){
	for(j=i;j<=N;j+=i){
		if(arr[j]==j)arr[j]=i;
	}
}	


Proof of complexity:
The sieve part is O(NloglogN) since it is very similar to the prime sieve.

For factorisation, at each step we divide n by arr[n]. Since arr[n]>=2, n is reduced to at least n/2 at every step. Hence, n will become 1 in at most O(logN) divisions.

Problem : http://www.spoj.com/problems/FACTCG2/


Finding primes in a range : The Segmented Sieve
The sieve of Eratosthenes is indeed a good approach to find the primes. However, if N is a very large number , its space complexity will be O(N) which may not fit in cache memory. 
Using segmented sieve, we can do the same with a O(sqrt(N)) space ,the time complexity remaining the same .  
In segmented sieve , we use the simple sieve for calculating primes up to sqrt(N).
For the other remaining elements , the order in which we mark off the primes is changed. Instead of marking all multiples of a prime p in range (p,N] , we proceed by marking the multiples of p for data that will fit in our memory and then continue for the next set of data.
In simple words ,say if limit of data that can fit in memory is B . Then we will apply simple sieve for all elements upto sqrt(N) and then apply segmented sieve. We will find primes in range [sqrt(N), sqrt(N)+B) , then [sqrt(N)+B, sqrt(N)+2*B) and so on. 
The segmented sieve also comes in handy when we need to find primes in the range [L,R] and R is large enough but we can create an array of size [R-L+1].

Problem : http://www.spoj.com/problems/PRINT/
The problem PRIME1 can also be solved using segmented sieve to get a better running time.

Finding sum of divisors of all numbers upto n

A naive approach would be to find all the divisors of i (1<= i<= n) which will take O(sqrt(i)) time for each i . The loose upper bound for this will be O(n3/2) .
Better approach :
Let Sum[i] denote the sum of all divisors of i. Sum[i] is initialised to 0.
Then for all j from 1 to n, we check for the multiples of j (say i ) and add j to Sum[i] ,i.e for all multiples of j, increment their sum of divisors , Sum by j.

__code__<br>
 int Sum[n+1];
 for (int i=1; i<=n; i++)
	Sum[i] = 0;                        
 for (int j=1; j<=n; j++)
	for(int i=j; i<=n; i+=j)
		Sum[i] += j;

 For each j , the inner loop iterates n/j times . The total time required is (n/1 + n/2 + n/3 + … + n/n) = n(1 + ½+ ⅓ + …+ 1/n ). The time complexity for above code is O(nlogn).

Problem : http://www.spoj.com/problems/DIVSUM/
Problem : http://www.spoj.com/problems/NDIV/
Problem : http://www.spoj.com/problems/NFACTOR/
 
<b>Greatest Common Divisor (GCD)</b>

Greatest Common Divisor or GCD of two numbers is defined as the largest positive number that divides both the numbers.
gcd(a,b) = {max(d) : d|a,d|b}

A brute-force approach to find the GCD of two numbers would be iterating from the lower number to 1. The first number found to divide both numbers would be the GCD.
 
__code__<br>
int GCD (int a, int b)
{
        	int res, m =min(a,b);
        	for (int i=m;i>0;i--)
{
        	if (a%i==0 && b%i==0)
{
res=i;
return res;
}
}
}


The time complexity of this function is O(min(a,b)). This can be improved by using what is known as the Euclid’s Algorithm for GCD which states GCD (A,B) = GCD(B,A%B).

Say, for A= 2177 and B= 147
GCD (2177,147) = GCD (147,2177%147) = GCD(147,119)
		= GCD (119,147%119) = GCD(119,28)
		=GCD (28,119%28) = GCD(28,7)
		=GCD( 7,28%7) = GCD(7,0) = 7
GCD of a positive integer and 0 is the positive integer itself.

So GCD is computed as :
__code__<br>
int GCD (int a,int b)
{
	if (b==0) return a;
	return GCD(b, a%b);
}


Problem :  <a href="http://www.spoj.com/problems/COMDIV">COMDIV</a>

<b>Extended Euclid Algorithm</b>

Equations of the form Ax + By = C have an integer solution if and only if C is a multiple of GCD(A,B). This is easy to prove as the LHS is always divisible by GCD(A,B). 
So, effectively if we find a solution for Ax + By = GCD(A,B) say (x0,y0) the solution for Ax+By = C
will be (Cx0/GCd(A,B),Cy0/GCd(A,B)).
Moreover, GCD(A,B) has a special property (Bezout’s Identity) that it can always be represented in the form of an equation, i.e., Ax + By = GCD(A, B). The Extended Euclidean Algorithm uses this fact to find integer solutions for the equation Ax+By = GCD (A,B). This algorithm gives us the coefficients (x and y) of this equation which will be later useful in finding the Modular Multiplicative Inverse. These coefficients can be zero or negative in their value.

Implementation:
__code__<br>
Int d, x, y;
void extendedEuclid(int A, int B) 
{
if(B == 0)
{
        		d = A;
        		x = 1;
        		y = 0;
    	}
    	else 
{
        		extendedEuclid(B, A%B);
        		int temp = x;
        		x = y;
	        	y = temp - (A/B)*y;
    	}
}
//d = GCD(A,B)
//Ax + By = d

If x and y (any solution) are known then general solution X, Y can be calculated as follows.
X = x + k*(b/GCD(a,b))
Y = y - k*(a/GCD(a,b))
The Time Complexity of Extended Euclid’s Algorithm is O(log(max(A,B))).

Problem : https://www.hackerearth.com/challenge/competitive/__code__<br>monk-number-threory-part-1/algorithm/monk-and-fredo-cm-number-theory/

Euler’s Totient Function

Euler’s Totient Function (represented by phi(n) or 𝛷(n)) is a very important number theoretic function with a lot of useful properties. It is the count of numbers less than or equal to n and coprime to n. More formally 

𝛷(n) =Count{ i : 1<=i<=n and gcd(i,n) =1}

The function has the following properties :
 
𝛷(n) is a multiplicative function i.e if m and n are coprime  𝛷(mn) =𝛷(m)𝛷(n)
If p is a prime, 𝛷(p) = p-1
For any prime p and natural number k, 𝛷(p^k)  = p^k-p^(k-1) = p^k(1-1/p)

We can use these properties to make the computation of 𝛷(n) easier . If
 n = p1^a1 x … x pk^ak 
𝛷(n)     = 𝛷(p1^a1 x … x pk^ak )
	=  𝛷(p1^a1) 𝛷(p2^a2) … 𝛷(pk^ak)
	= (p1^a1(1-1/p1)) (p2^a2(1-1/p2)) … (pk^ak(1-1/pk))
𝛷(n) = n {pi|n}(1-1/pi)

The SPOJ Problem ETF  requires us to calculate 𝛷(n) for n in the range [1,10^6] for T test cases.
A naïve approach for this problem can be:
 
__code__<br>
int phi (int n)
{
            	int res=0;
            	for (int i=1;i<=n;i++)
                            	if( gcd(i,n)==1 ) res++;
            	return res;
}

 
But under the given constraints it will give TLE as the complexity would be O(T*NlogN).
 
Instead, using the properties we can calculate 𝛷(n) with a factorisation algorithm :
 
__code__<br>
int phi(int n)
{
	int a=n;
	for(int i=2;i*i<=n;i++)
	{
    		if(n%i==0) a-=a/i;
    		while(n%i==0) n/=i;
	}
	if(n>1) a-=a/n;
	return a;
}

This solves the problem in O(T*sqrt(N)) time.

Can we do better? Yes we can. We can compute the euler totient function of all numbers from 1 to N in O(NlogN) complexity and then answer each test case using the precalculated value in O(1). This reduces the overall complexity of the problem to O(NlogN + T).

__code__<br>
for(i=1;i<=N;i++)phi[i]=i;

for(i=2;i<=N;i++)
if(phi[i]==i){	// This condition guarantees that i is prime since it was not modified by any number <i.
		for(j=i;j<=N;j+=i)
		phi[j]-=(phi[j]/i);		// In this step we multiply (1-1/i) to each multiple of i, since it would be a prime factor in factorisation of all its multiples.
}


Similar to the segmented sieve for primes, we can also design a segmented sieve for the Euler totient function. This is left as an exercise for the reader.

Problem : http://www.spoj.com/problems/ETFS/
Problem : http://www.spoj.com/problems/ETFD/


<b>Fermat’s Little Theorem</b>

This theorem states that for a prime p and a natural number a coprime to p

a^(p-1) ≡ 1 (mod p)

Euler’s Theorem

This is the generalization of Fermat’s Little Theorem. It states that for coprime numbers a and n

a^𝛷(n) ≡ 1 (mod n)



<b>Fibonacci Numbers</b>
Mathematically the nth Fibonacci number is defined as
 Fn = 	0, n = 0
1, n = 1
 Fn-1 + Fn-2, n >=2
The sequence looks like this:
{0, 1, 1, 2, 3, 5, 8, 13, 21…}

Some Properties
1.	Fm+n = Fm · Fn+1 + Fm-1 · Fn

2.	Fn = (𝛷n - (1 - 𝛷)n) / √5
𝛷= (1+√5)/2
(Due to precision errors we don’t use this in code)

3.	F1 + F2 +···+ Fn =  Fn+2 − 1.

4.	Fn = n−1C0 + n−2C1 + n−3C2 + …

5.	F12 + F22 +···+ Fn2  = Fn.Fn+1

6.	F1 + F3 +···+ F2n+1 = F2n+2 ,	1 + F2 + F4 +···+ F2n = F2n+1

7.	F1.F2 + F2.F3 + … + F2n-1.F2n = F2n2

8.	gcd(Fm, Fn) = Fgcd(m,n)

9.	m|n ⇒ Fm|Fn


To compute all fibonacci numbers less than a given number N
This can be done with a time complexity O(N) using the recursive definition.

To compute a single Fibonacci Number, the Nth in series
We can find FN in O(log(N)) this way:
Consider the 2x2 matrix X
X	 = 	1 	1
		1 	0
Now,
XN  	= 	FN+1	FN
		FN	FN-1, 	N>=1
This formula can be proven using induction.
Hence, to solve our original problem we can find  XN, for which the pseudo code is same as exponentiation of any number(which is O(log(N))) except the values are going to be matrices for this problem and the operations addition and multiplication on matrices need to be defined as functions with input and output as 2x2 arrays.

Problem : http://www.spoj.com/problems/FIBOSUM/


More problems for the reader’s practice :
http://www.spoj.com/problems/MAIN74/
http://www.spoj.com/problems/INVPHI/
http://www.spoj.com/problems/DCEPC200/
http://www.spoj.com/problems/GGD/
http://www.spoj.com/problems/POWFIB/
https://projecteuler.net/problem=72
https://projecteuler.net/problem=69
https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1031
https://uva.onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1574


