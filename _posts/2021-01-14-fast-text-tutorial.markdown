---
layout : post
title : Fast Text Tutorial
date : 2021-01-14 16:23:27 +0530
Categories : Fast Text
---

### What is FastText?

FastText is an open-source, free, lightweight library that allows users to learn text representations and text classifiers. It works on standard, generic hardware. Models can later be reduced in size to even fit on mobile devices.

### How to use Fast Text?

We use fast text either as a commandline tool or python module. In this tutorial, I will take you through fast-text as a puthon module.

Clone the Github [Repository](https://github.com/facebookresearch/fastText) into your local machine to start working with it.

git clone https://github.com/facebookresearch/fastText.git

once you clone, go to the fastText directory and install the required packages

sudo pip install .

Now, we are ready to use but confirm if the set-up was successful. You can do that by importing `fasttext` using import fasttext.


### Getting Started

There is a help function here which basically works as a manual/documentation at a very high level

so, try `help(fasttext.FastText)`

In this tutorial, I will try my best to use all the possible functionalities available in fastText but our prime focus will be on `train_supervised`

### Getting and Organizing data

As a part our ritual, we need labeled data to train our supervised classifier. We are taking the data from stack-exchange.com . Download [here](https://dl.fbaipublicfiles.com/fasttext/data/cooking.stackexchange.tar.gz)

unzip and you will find a folder name `cooking.stackexchange`.

Now, we have all the data. The next job on our hand is to split data into training and validation sets. 

let us create files cooking.train and cooking.valid:

wc cooking.stackexchange.txt

gives the number of datapoints in dataset. let us split it into 80:20 ratio

head -n 12404 cooking.stackexchange.txt > cooking.train
tail -n 3000 cooking.stackexchange.txt > cooking.valid

### Training our classifier

{% highlight ruby %}
>>> import fasttext
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train")
Read 0M words
Number of words:  14543
Number of labels: 735
Progress:   9.3% words/sec/thread:   93836 lr:  0.090737 avg.loss: 16.653322 ETA
Progress:  18.5% words/sec/thread:   94633 lr:  0.081536 avg.loss: 13.589425 ETA
Progress:  27.2% words/sec/thread:   93737 lr:  0.072765 avg.loss: 12.150671 ETA
Progress:  36.4% words/sec/thread:   93749 lr:  0.063581 avg.loss: 11.399540 ETA
Progress:  45.6% words/sec/thread:   93663 lr:  0.054355 avg.loss: 10.992249 ETA
Progress:  54.6% words/sec/thread:   93759 lr:  0.045447 avg.loss: 10.696486 ETA
Progress:  63.9% words/sec/thread:   93912 lr:  0.036141 avg.loss: 10.480530 ETA
Progress:  72.9% words/sec/thread:   93727 lr:  0.027138 avg.loss: 10.335073 ETA
Progress:  82.2% words/sec/thread:   93788 lr:  0.017838 avg.loss: 10.187409 ETA
Progress:  91.5% words/sec/thread:   93885 lr:  0.008476 avg.loss: 10.105961 ETA
Progress: 100.0% words/sec/thread:   93142 lr: -0.000022 avg.loss: 10.047604 ETA
Progress: 100.0% words/sec/thread:   93105 lr:  0.000000 avg.loss: 10.047604 ETA:   
0h 0m 0s
{% endhighlight %}

The `input` argument indicates the file containing the training examples.

so we have our model tarined in `model` variable. We can also call `save_model` to save it as a file and load it later with `load_model` function. 
{% highlight ruby %}
model.save_model("model_cooking.bin")
{% endhighlight %}
Let's play around now with our `model`:

We are good to predict based on the training we did. Let's do it:
{% highlight ruby %}
>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.09332366]))
>>> model.predict("is it safe to go to an restaurant?")
(('__label__food-safety',), array([0.09150641]))
{% endhighlight %}
Did you observe?: Adding `?` changed the prediction value a bit but the prediction label is same. With this we can expect our model to perform bad under certain scenario. So we need to improve our model. Let's dig deep into that now.


### Making the model Accurate:

The first thing that comes to my mind is why can't we pre-process the data. Like remove unnecessry spaces, special characters, and doing some cool pre-processing stuff like identifying `@` as `a`. It's cool but we shall not do that now.
{% highlight ruby %}
>>> cat cooking.stackexchange.txt | sed -e "s/\([.\!?,'/()]\)/ \1 /g" | tr "[:upper:]" "[:lower:]" > cooking.preprocessed.txt
>> head -n 12404 cooking.preprocessed.txt > cooking.train
>> tail -n 3000 cooking.preprocessed.txt > cooking.valid
{% endhighlight %}
Now, let's train the model
{% highlight ruby %}
>>> import fasttext
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train")
Read 0M words
Number of words:  8952
Number of labels: 735
Progress:   9.0% words/sec/thread:  102245 lr:  0.090960 avg.loss: 16.451744 ETA
Progress:  18.5% words/sec/thread:  103504 lr:  0.081533 avg.loss: 13.291600 ETA
Progress:  27.4% words/sec/thread:  102971 lr:  0.072622 avg.loss: 11.978544 ETA
Progress:  36.4% words/sec/thread:  102884 lr:  0.063637 avg.loss: 11.299486 ETA
Progress:  45.6% words/sec/thread:  102598 lr:  0.054429 avg.loss: 10.896181 ETA
Progress:  54.9% words/sec/thread:  102546 lr:  0.045095 avg.loss: 10.606382 ETA
Progress:  63.8% words/sec/thread:  102619 lr:  0.036166 avg.loss: 10.397550 ETA
Progress:  72.9% words/sec/thread:  102525 lr:  0.027128 avg.loss: 10.235602 ETA
Progress:  82.2% words/sec/thread:  102566 lr:  0.017795 avg.loss: 10.103518 ETA
Progress:  91.1% words/sec/thread:  102545 lr:  0.008864 avg.loss: 10.017081 ETA
Progress: 100.0% words/sec/thread:  102544 lr: -0.000008 avg.loss:  9.952509 ETA
Progress: 100.0% words/sec/thread:  102519 lr:  0.000000 avg.loss:  9.952509 ETA:   
0h 0m 0s
{% endhighlight %}
You can clearly see the number of words came down from `14543` to `8952`. This means our pre-processing combined various forms of same word.

let's predict the same sentences as earlier 
{% highlight ruby %}
>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.11207097]))
>>> model.predict("is it safe to go to an restaurant?")
(('__label__food-safety',), array([0.15131585]))
{% endhighlight %}
You can clearly see the prediction value has gone up significantly.

How can we even improve the performace?

Altering the learning rate and epoch,batch size??

### More epoch

By default, fastText sees each training example only five times during training, which is pretty small, given that our training set only have 12k training examples.
{% highlight ruby %}
>>> import fasttext
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train",epoch=25)
{% endhighlight %}
let's predict now:
{% highlight ruby %}
>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.43556383]))
>>> model.predict("is it safe to go to an restaurant?")
(('__label__food-safety',), array([0.32207996]))

>>> model.test("/path/to/directory/cooking.stackexchange/cooking.valid") 
(3000, 0.518, 0.22401614530776992)
{% endhighlight %}
```
Here, 3000 is the size of sample we tested on. 0.518 is the precision and 0.224 is the recall rate. 
The precision is the number of correct labels among the labels predicted by fastText. The recall is the number of labels that successfully were predicted, among all the real labels.

Example: 
model.predict("is it safe to go to an restaurant?",k=5)
(('__label__food-safety', '__label__beef', '__label__storage-method', '__label__chicken', '__label__storage-lifetime'), array([0.67451811, 0.05916279, 0.02460619, 0.02096862, 0.02020765])) 

the query has 2 real labels(assumption)

Thus, one out of five labels predicted by the model is correct, giving a precision of 0.20. Out of the two real labels, only one is predicted by the model, giving a recall of 0.50.
```
see, the numbers they are raising , which anyways is a good sign we are training the model good.

Now, let's play with learning rate

### Learning Rate

Learning rate corresponds to how much the model changes after processing each example. A learning rate of 0 would mean that the model does not change at all, and thus, does not learn anything. Good values of the learning rate are in the range 0.1 - 1.0

{% highlight ruby %}
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train", lr=1.0 ,epoch=25)

>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.63764799]))
>>> model.predict("is it safe to go to an restaurant?")
(('__label__food-safety',), array([0.67451811]))
{% endhighlight %}
Just wow!! prediction value is getting bigger and bigger
{% highlight ruby %}
>>> model.test("/path/to/directory/cooking.stackexchange/cooking.valid") 
(3000, 0.585, 0.25299120657344676)
{% endhighlight %}
Now, let's try with the combination of words as one. In other words, use word n-grams instead of uni-grams used.

### Word N-grams
{% highlight ruby %}
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train", lr=1.0 ,epoch=25,wordNgrams=2)

>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.96595222]))
{% endhighlight %}
96% prediction value is toomn good to have. However, let's test on validation set.
{% highlight ruby %}
>>> model.test("/path/to/directory/cooking.stackexchange/cooking.valid") 
(3000, 0.6026666666666667, 0.26063139685743114)
{% endhighlight %}
Now, we have achieved 60% precision. It is good for a beginner.

### using loss fucntions of our choice:

let try using Hierarchical softmax. we can enable this using loss = 'hs'. let's try this out with `bucket=200000, dim=50`
{% highlight ruby %}
>>> model.predict("is it safe to go to an restaurant")
(('__label__food-safety',), array([0.85158682]))
{% endhighlight %}
Interesting, performance, went down.
{% highlight ruby %}
>>> model.test("/path/to/directory/cooking.stackexchange/cooking.valid") 
(3000, 0.5916666666666667, 0.2558742972466484)
{% endhighlight %}
When we want to assign a document to multiple labels, we can still use the softmax loss and play with the parameters for prediction, namely the number of labels to predict and the threshold for the predicted probability. However playing with these arguments can be tricky and unintuitive since the probabilities must sum to 1.

A convenient way to handle multiple labels(Multi-label classification) is to use independent binary classifiers for each label. This can be done with -loss one-vs-all or -loss ova

### Multi-label classification
{% highlight ruby %}
>>> model = fasttext.train_supervised(input="/path/to/directory/cooking.stackexchange/cooking.train", lr=0.5 ,epoch=25,wordNgrams=2, bucket=200000, dim=50, loss="ova")

>>> model.predict("is it safe to go to an restaurant",k=-1,threshold=0.5)
(('__label__food-safety',), array([0.99864298]))

>>> model.test("/path/to/directory/cooking.stackexchange/cooking.valid") 
(3000, 0.6056666666666667, 0.2619287876603719)

{% endhighlight %}

I'll try to im provise and take precision and recall and continue the blog later some-time.


### References
[fasttext](https://fasttext.cc/docs/en/unsupervised-tutorial.html) is the reference and this tutorial is written as part of learn by writing philosophy


