---
layout: post
title:  "NumPy features in short"
date:   2021-01-06 16:23:27 +0530
categories: learn AI
---

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

