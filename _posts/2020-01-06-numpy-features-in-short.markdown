---
layout: post
title:  "NumPy features in short"
date:   2021-01-06 16:23:27 +0530
categories: learn AI
---

`Basics of NumPy ndarray`

`ndim`: Gives the number of dimensions

`shape`: gives the shape of the ndarray

`size`: gives the number of entries in the ndarray(i.e product of elements in shape)

`dtype`: gives the type of array

`itemsize`: gives the size of the datatype

`let us import numpy as np`

`np.zeros(m,n)`: gives a matrix of size mxn with all zeroes
`np.ones(m,n)`: gives a matrix of size mxn with all ones

`np.arange(m)`: gives a list of size m from 0 to m-1

`np.arange(i,f,s)`: gives a list of elements starting from i and with a step of s

`np.ndarray.reshape(a1,...ai,....an)` : reshapes the already existing ndarray. if any one ai is -1. it's size is deciding based on the size of teh ndarray

`np.linspace(i,f, n)`: gives n points between i and f which are equi-distant

`Operations on Matrices`:

`element wise multiplication:` M1 * M2

`matrix multiplication:` M1 @ M2 or M1.dot(M2)

`+= and *=` are allowed but with dtype compatability as in any different language

`max,min,sum`
{% highlight ruby %}
>>>a
array([[0.82770259, 0.40919914, 0.54959369],
       [0.02755911, 0.75351311, 0.53814331]])
>>> a.sum()
3.1057109529998157
>>> a.min()
0.027559113243068367
>>> a.max()
0.8277025938204418
{% endhighlight %}

when argument like axis=0 or 1 is mentioned the arrays gets collapsed along the mentioned axis and the operation happens along the mentioned axis


{% highlight ruby %}
>>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>>>
>>> b.sum(axis=0)                            
array([12, 15, 18, 21])
{% endhighlight %}


`Math functions`

`np.sqrt`:return the square root matrix of the given matrix

`np.exp():` return the exponential matrix of the given matrix 

`For Loop`
{% highlight ruby %}
for row in b:
	print(row)  # prints every row


for element in b.flat:
	print(element)     #flattens out the ndarray



a.ravel()              #flattens out the ndarray

a.T                    #returns transpose of a matrix	

a.resize(m,n)          #returns the resized matrix and happens in-place
{% endhighlight %}


Stacking 

{% highlight ruby %}
with vstack arrays are concatenated along axis 0

with hstack arrays are concatenated along axis 1

The function column_stack stacks 1D arrays as columns into a 2D array. It is equivalent to hstack only for 2D arrays

The function row_stack stacks similar to as vstack



{% endhighlight %}


`np.hsplit(a,(3,4))`: returns arrays split at columns 3,4 resulting into 3 arrays


Making Copies and Deep copy

{% highlight ruby %}
Simple assignments make no copy of objects

c = a.view() # c is a view of data owned by a i.e  c.flags.owndata return False

Slicing an array returns a view of it

d = a.copy()      #creates a mew ndarray same as a

this is helpful When we need some section of very huge data. we take a copy of that specific data and delete the big-data


{% endhighlight %}

`Linear Algebra`:

{% highlight ruby %}

a.transpose() # returns transpose of a matrix

np.linalg.inv(a)  # returns inverse of a matrix

np.eye(n)         #returns an identity matrix of size nxn

np.trace(a)       #returns the trace of matrix

np.linalg.solve(a, y)  # solves for x in the expression ax=y

`np.linalg.eig(j)       #  I am yet to explore this `

{% endhighlight %}

Here is how we define a function and implement

{% highlight ruby %}
import numpy as np 

c = np.arange(12).reshape(3,4)

def softmax(vector):
	e = np.exp(vector)
	return e/e.sum()


def attention(Q,K,V):
	x = Q @ np.transpose(K)/np.sqrt(K.ndim)
	sm = softmax(x) @ V
	return sm

print(attention(c,c,c))
{% endhighlight %}

