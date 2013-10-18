---
layout: post
title: "SPOJ NACCI"
date: 2013-08-28 20:00
comments: true
categories: [Editorial, SPOJ, Question of the Week, Matrix Exponentation]
---

Prerequisites : Matrix Exponentation.

[Problem](http://www.spoj.com/problems/NACCI/) : 

Find f[M] such that if (M > N) f[M] = sum(f[M - i]) for(i = 1 to N) else f[M] = M.

Explaination :

For convenience let us take k = 4. So f[n] = f[n - 1] + f[n - 2] + f[n - 3] + f[n - 4].
Let us consider two matrices A,B of size (4 X 4)

Matrix A :
    
    { f[i]  f[i - 1]  f[i - 2] f[i - 3] }
    {  0        0        0         0    }
    {  0        0        0         0    }
    {  0        0        0         0    }
    
Matrix B :
    
    { 1   1   0   0 }
    { 1   0   1   0 }
    { 1   0   0   1 }
	{ 1   0   0   0 }

MAtrix A X B :

    { (f[i]+f[i-1]+f[i-2]+f[i-3]   f[i]   f[i - 1]   f[i - 2] }
    {  0                             0        0         0     }
    {  0                             0        0         0     }
    {  0                             0        0         0     }

That will be equal to

    { (f[i+1]   f[i]   f[i - 1]   f[i - 2] }
    {    0        0        0         0     }
    {    0        0        0         0     }
    {    0        0        0         0     }		

So multiplying matrix A by matrix B by p times leaves f[i+p] in the first row first column position.
Let initially Matrix A be

    { f[3]     f[2]     f[1]    f[0] }
    {  0        0        0       0   }
    {  0        0        0       0   }
    {  0        0        0       0   }

So Multiplying A with B by p times leaves f[3+p] in the first row first column element of product matrix.

To get M'th element we just need to multiply A with B by M - 3 times. As M is very large we can calculate this my repeated squaring in O(logM) multiplications. For a (N X N) matrix as each matrix multiplication takes O(N^3) steps overall complexity becomes O(log(M)*(N^3)).

For an (N X N) matrix, B might look as 

Matrix B :

	{ 1  1  0  0  0 ...}
	{ 1  0  1  0  0 ...}
    { 1  0  0  1  0 ...}
	{ 1  0  0  0  1 ...}  
	{ .................}
    

(So on till N rows N columns)

To be generalized let b(i,j) be i'th row j'th column element of matrix. Then b(i,j) = 1 if (j == 0) or (j == i + 1)
In general any linear equation of above type like f[n] = a1*f[n - 1] + a2*f[n - 2] + ... can be solved by making B[i][0] = ai for all (i = 0 to N - 1)

PseudoCode :


	int[][] MatrixMul(int[][] A, int[][] B)

    	int[][] P;
    
    	int i,j,k;
	
	    for(k = 0 to n - 1)
	
    	    for(i = 0 to n - 1)
		
        	    for(j = 0 to n - 1)
			
            	    P[i][j] += A[i][k] * B[k][j]
			
            	end loop
		
    	    end loop

    	end loop
	
    	return P

	end MatrixMul


	int[][] PowerMatrix(int[][] B , power) :

    	if(power == 1) :
		
        	return B
	
    	else
	
        	int[][] temp = PowerMatrix(B , power / 2) 
		
        	temp = MatrixMul(temp,temp)
		
    	    if(power % 2 == 1) :
		
        	    temp = MatrixMul(temp,B)
		
    	    end if
		
        	return temp
	
    	end ifelse
    
    end PowerMatrix


	int NACCI(int M , int N) :
	
    	if M < N
		
        	return M
		else

        	int A[N][N],B[N][N],i,j;
		
        	for(j = 0 to N - 1)
		
            	A[0][j] = N - 1 - j
        
       	 	end loop
		
    	    for(i = 1 to N - 1)
			
        	    for(j = 0 to N - 1)
			
            	    A[i][j] = 0
			
           		end loop
		
        	end loop
		
        	for(i = 0 to N - 1)
		
            	B[i][0] = 1
			
            	for(j = 1 to N - 1)
			
    	            if (j == i + 1)
				
        	            B[i][j] = 1
				
            	    else 
				
                	    B[i][j] = 0
				
                	end ifelse
			
            	end loop
	
        	end loop
		
    	    A = MatrixMul( A , PowerMatrix(B,M - N + 1) )
		
        	retrurn A[0][0]
	
    	end ifelse
	
	end NACCI

