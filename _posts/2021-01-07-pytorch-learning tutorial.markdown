---
layout: post
title:  "PyTorch Learning"
date:   2021-01-06 16:23:27 +0530
categories: learn AI
---

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


<h1>{{"Why PyTorch"}} </h1>
1) It used GPU and other accelarators to power up computations 50x.
2) Automatic differentiation Library for Nueral Networks.

Let's Learn PyTorch with an example of fitting a sin curve on a 3 degree polynomial.

NumPy approach: 
{% highlight ruby %}

import numpy as np
import math

x = np.linspace(-math.pi, math.pi, 2000)
y = np.sin(x)

a = np.random.randn()
b = np.random.randn()
c = np.random.randn()
d = np.random.randn()

learning_rate = 1e-6
for t in range(2000):
    # Forward pass: compute predicted y
    # y = a + b x + c x^2 + d x^3
    y_pred = a + b * x + c * x ** 2 + d * x ** 3

    # Compute and print loss
    loss = np.square(y_pred - y).sum()
    if t % 100 == 99:
        print(t, loss)

    # Backprop to compute gradients of a, b, c, d with respect to loss
    grad_y_pred = 2.0 * (y_pred - y)
    grad_a = grad_y_pred.sum()
    grad_b = (grad_y_pred * x).sum()
    grad_c = (grad_y_pred * x ** 2).sum()
    grad_d = (grad_y_pred * x ** 3).sum()

    # Update weights
    a -= learning_rate * grad_a
    b -= learning_rate * grad_b
    c -= learning_rate * grad_c
    d -= learning_rate * grad_d

print(f'Result: y = {a} + {b} x + {c} x^2 + {d} x^3')
{% endhighlight %}

Now, we try to implement this using tensors. Here we mention the device which the tensor should be loaded on for performance.
{% highlight ruby %}

import torch
import math


dtype = torch.float
device = torch.device("cpu") # device = torch.device("cuda:0") # Uncomment this to run on GPU

x = torch.linspace(-math.pi, math.pi, 2000, device=device, dtype=dtype)
y = torch.sin(x)

a = torch.randn((), device=device, dtype=dtype)
b = torch.randn((), device=device, dtype=dtype)
c = torch.randn((), device=device, dtype=dtype)
d = torch.randn((), device=device, dtype=dtype)

learning_rate = 1e-6
for t in range(2000):
    # Forward pass: compute predicted y
    y_pred = a + b * x + c * x ** 2 + d * x ** 3

    # Compute and print loss
    loss = (y_pred - y).pow(2).sum().item()
    if t % 100 == 99:
        print(t, loss)

    # Backprop to compute gradients of a, b, c, d with respect to loss
    grad_y_pred = 2.0 * (y_pred - y)
    grad_a = grad_y_pred.sum()
    grad_b = (grad_y_pred * x).sum()
    grad_c = (grad_y_pred * x ** 2).sum()
    grad_d = (grad_y_pred * x ** 3).sum()

    # Update weights using gradient descent
    a -= learning_rate * grad_a
    b -= learning_rate * grad_b
    c -= learning_rate * grad_c
    d -= learning_rate * grad_d


print(f'Result: y = {a.item()} + {b.item()} x + {c.item()} x^2 + {d.item()} x^3')


{% endhighlight %}

As mentioned earlier, PyTorch provides us the automatic differentiation also called auto-grad in short.

It is pretty simple to use in practice. `Each Tensor represents a node in a computational graph`. If x is a Tensor that has x.requires_grad=True then `x.grad` is another Tensor holding the gradient of x with respect to some scalar value.

So, we can take a breather by using this auto-grad provided by PyTorch to caluculate the complex back-propagation steps.

Now, Think which part of the above code is going to change.

1) Since a,b,c and d are learnable tensors, they must be provided with an attribute requires_grad as true.

2) Once a,b,c,d requires_grad as true, PyTorch automatically caluculates the all the gradiants required for back-propagation.

This is done by just calling `loss.backward()`. Now we have gradiants of a,b,c and d in a.grad,b.grad,c.grad and d.grad.
So we update our our learnable parameters as in previous cases and set the grad attributes of the learnable tensors to None for re-use.

So our code looks like this:
{% highlight ruby %}
import torch
import math

dtype = torch.float
device = torch.device("cpu") # device = torch.device("cuda:0")  # Uncomment this to run on GPU

x = torch.linspace(-math.pi, math.pi, 2000, device=device, dtype=dtype)
y = torch.sin(x)


a = torch.randn((), device=device, dtype=dtype, requires_grad=True)
b = torch.randn((), device=device, dtype=dtype, requires_grad=True)
c = torch.randn((), device=device, dtype=dtype, requires_grad=True)
d = torch.randn((), device=device, dtype=dtype, requires_grad=True)

learning_rate = 1e-6
for t in range(2000):
    # Forward pass: compute predicted y using operations on Tensors.
    y_pred = a + b * x + c * x ** 2 + d * x ** 3

    # Compute and print loss using operations on Tensors.
    # Now loss is a Tensor of shape (1,)
    # loss.item() gets the scalar value held in the loss.
    loss = (y_pred - y).pow(2).sum()
    if t % 100 == 99:
        print(t, loss.item())

    # Use autograd to compute the backward pass. This call will compute the
    # gradient of loss with respect to all Tensors with requires_grad=True.
    # After this call a.grad, b.grad. c.grad and d.grad will be Tensors holding
    # the gradient of the loss with respect to a, b, c, d respectively.
    loss.backward()

    # Manually update weights using gradient descent. Wrap in torch.no_grad()
    # because weights have requires_grad=True, but we don't need to track this
    # in autograd.
    with torch.no_grad():
        a -= learning_rate * a.grad
        b -= learning_rate * b.grad
        c -= learning_rate * c.grad
        d -= learning_rate * d.grad

        # Manually zero the gradients after updating weights
        a.grad = None
        b.grad = None
        c.grad = None
        d.grad = None

print(f'Result: y = {a.item()} + {b.item()} x + {c.item()} x^2 + {d.item()} x^3')

{% endhighlight %}









