---
layout: post
title:  "PyTorch Learning"
date:   2021-01-06 16:23:27 +0530
categories: learn AI
---

`PyTorch Learning`

<h1>{{"Why PyTorch"}} </h1>
1) It used GPU and other accelarators to power up computations 50x.
2) Automatic differentiation Library for Nueral Networks.

<h1>{{"Tensors"}}</h1>
Tensors are data structures similar to arrays and matrices. PyTorch uses tensors to encode the inputs, outputs and paramaters of the model. These Tensor run on GPU or specific hardware devices to improvise the performance

For the rest to tutorial lets follow the below:


{% highlight ruby %}

import torch
import numpy as np

{% endhighlight %}

Initialising a tensor:

Let 

data = [[1, 2],[3, 4]]

Initializing directly from data
{% highlight ruby %}
x_data = torch.tensor(data)
{% endhighlight %}

Initializing using numpy
{% highlight ruby %}
np_array = np.array(data)
x_np = torch.from_numpy(np_array)
{% endhighlight %}

Initilaising using another tensor
{% highlight ruby %}
x_ones = torch.ones_like(x_data) # retains the properties of x_data

x_rand = torch.rand_like(x_data, dtype=torch.float) # overrides the datatype of x_data
{% endhighlight %}


<h1>{{"Attributes of Tensors"}}</h1>

tensor = torch.rand(3,4)   #assume

Shape of tensor: tensor.shape
{% highlight ruby %}
torch.Size([3, 4])
{% endhighlight %}

Datatype of tensor: tensor.dtype
{% highlight ruby %}
torch.float32
{% endhighlight %}

Device tensor is stored on: tensor.device
{% highlight ruby %}
cpu
{% endhighlight %}


<h1>{{"Operations opn tensors"}}</h1>
If GPU is available to be used we move the tensor to GPU
{% highlight ruby %}
if torch.cuda.is_available():
  tensor = tensor.to('cuda')
{% endhighlight %}

Indexing and Slicing are similar to numpy

`Joining matrices (or) Concatenating`
{% highlight ruby %}
torch.cat([tensor, tensor, tensor], dim=1)

The above statement returns : 
tensor([[1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.],
        [1., 0., 1., 1., 1., 0., 1., 1., 1., 0., 1., 1.]])
{% endhighlight %}
`Multiplication`

tensor * tensor or tensor.matmul(tensor) returns the same tensor that is formed as a result of element wise multiplication

`In-Place Operations`

In-place operations on tensors is simple. It is acheived by suffixing a function with `_` like `x.copy_(y), x.add_()`

<h1>{{"Bridge between numpy and Tensors"}}</h1>

creating a tensor from numpy array:

let 
{% highlight ruby %}
a = np.ones(3)
tensor = torch.from_numpy(a)
{% endhighlight %}

Any changes done on numpy array reflects on the tensor. This is due to the logic that both tensor and numpy array share the same memory locations. 

Let's see how a numpy array is created from tensor.
{% highlight ruby %}
tensor = torch.ones(3)
n = tensor.numpy()
{% endhighlight %}

Any changes done on tensor reflects on the numpy array . This is due to the logic that both tensor and numpy array share the same memory locations. 






