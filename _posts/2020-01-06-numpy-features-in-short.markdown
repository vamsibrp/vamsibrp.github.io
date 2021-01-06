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
>>> b.sum(axis=0)                            # sum of each column
array([12, 15, 18, 21])
{% endhighlight %}


`Math functions`

`np.sqrt`:return the square root matrix of the given matrix

`np.exp():` return the exponential matrix of the given matrix 

{% highlight ruby %}
>>> a = `np.arange(10)**3`
>>> a
array([  0,   1,   8,  27,  64, 125, 216, 343, 512, 729])
{% endhighlight %}

I have tried to code the attention function as a part of our reading group discussion:

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

