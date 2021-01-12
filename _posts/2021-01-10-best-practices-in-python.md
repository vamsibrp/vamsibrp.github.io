---
layout: post
title:  "Best Practices in Python"
categories: Learn AI

---

### Structuring The Project

In practical terms, “structure” means making clean code whose logic and dependencies are clear as well as how the files and folders are organized in the filesystem. In this section, we take a closer look at Python’s modules and import systems as they are the central elements to enforcing structure in your project.

### Structuring The Repository

Just as any other aspect of your healthy development cycle, Repository structure is also a crucial part of your project
Things people see when people visit your repo:

```
* Project Name
* Project Description
* File structures

```
We generally take care of Name and description but sometimes fail to maintain good structure. To avoid having massive dump of file on your repo, we need to maintain a proper folder structure in the Repository

Here are some of the key aspects of the respository:

### Actual Module
 Let there be 4 files which actually make the module. It is best to have them in a folder with generic name rather having them on the root directory of the Repo. If in-case there is only one such file better have that file on the root instead of folder.

### Licence
It is the second most important part of your repo after your code. This let's the other developers use or contribute your code basing on the licence. If you miss adding this, your code may sometimes not be usable by other developers owing to licence issues.

### ./Setup.py
 Package and distribution management.

### Requirement file
stored as ./requirement.txt present on the root of the repo. It specifies the pip dependencies required to contribute to the project: testing, building, and generating documentation. Refer [this](https://pip.pypa.io/en/stable/user_guide/#requirements-files) for wide insights. If your project has no development dependencies, or if you prefer setting up a development environment via setup.py, this file may be unnecessary.

### Documentation
saved as ./docs/ on root folder. It conatains information about Package Reference documentation

### Test Suite
stored in /tests/ on root folder. writes all the test cases and test our code. To test our code we need to import our modules into our test suite.

This can be done in two ways:
```
* Expect the package to be installed in site-packages.
* Use a simple (but explicit) path modification to resolve the package properly.
```
It is suggested to use method 2 

To give the individual tests import context, create a tests/context.py file:
{% highlight ruby %}
import os
import sys
sys.path.insert(0, os.path.abspath(os.path.join(os.path.dirname(__file__), '..')))
{% endhighlight %}
import sample
Then, within the individual test modules, import the module like so:
{% highlight ruby %}
from .context import sample
{% endhighlight %}
This will always work as expected, regardless of installation method.

PS: If you wish to learn more. Please [click](https://docs.python-guide.org/writing/tests/)


### MakeFile
Stored as ./MakeFile in root repo. It provides user with utilities to build or install packages basing on the arguments. 


### Modules 

Python modules are one of the main abstraction layers available . Abstraction layers allow separating code into parts holding related data and functionality. So, there arises a need to import one module into the other. This is done with the import and from `...` import statements. These modules may be in-built, third-party or from our own code.

PS: module names should be free off `.,_,!,` to make it short and readable

The best way to import a module is by importing the whole module instead of a particular fucntion or using `*` 

`Example:`

Approach A:
from modu import *
x = sqrt(4)

Approach B:
from modu import sqrt
x = sqrt(4)

Approach C:
import modu
x = modu.sqrt(4)

Among the three approaches, C > B > A because of the readability and re-usability of the code.

### Structure of code

It is relatively easy to structure a Python project. Easy, here, means that you do not have many constraints and that the module importing model is easy to grasp. This means it is also easy to do it poorly. Let us see some of the ways of poor code structures.

```
* Multiple and messy circular dependencies
* Hidden coupling
* Heavy usage of global state or context
* Spaghetti code
* Ravioli code
```

### Packages

Any directory with an __init__.py file is considered a Python package. The different modules in the package are imported in a similar manner as plain modules, but with a special behavior for the __init__.py file, which is used to gather all package-wide definitions.

Let's assume A file modu.py in the directory pack/ and it is imported with the statement import pack.modu. This statement will look for __init__.py file in pack and execute all of its top-level statements. Then it will look for a file named pack/modu.py and execute all of its top-level statements.

Leaving an __init__.py file empty is considered normal and even good practice


### OOP

Yes, Python implements OOP concepts but not as main programming paradigm. The way Python handles modules and namespaces gives the developer a natural way to ensure the encapsulation and separation of abstraction layers. So, python programmers tend not to use OOP unless business requirement comes in.

There are some reasons to avoid unnecessary object-orientation. The problem, as pointed out by the discussions about functional programming, comes from the “state” part of the equation. In some architectures, typically web applications, multiple instances of Python processes are spawned as a response to external requests that happen simultaneously. In this case, holding some state in instantiated objects, which means keeping some static information about the world, is prone to concurrency problems or race conditions.

Sometimes, between the initialization of the state of an object (usually done with the __init__() method) and the actual use of the object state through one of its methods, the world may have changed, and the retained state may be outdated. This and other issues led to the idea that using stateless functions is a better programming paradigm.

A function’s implicit context is made up of any of the global variables or items in the persistence layer that are accessed from within the function. A function’s implicit context is made up of any of the global variables or items in the persistence layer that are accessed from within the function

Carefully isolating functions with context and side-effects from functions with logic(called pure functions) allows the following benefits. In summary, pure functions are more efficient building blocks than classes and objects for some architectures because they have no context or side-effects.


### Decorators



### Context Managers


### Dynamic typing

Python is dynamically typed, which means that variables do not have a fixed type. The dynamic typing of Python is often considered to be a weakness, and indeed it can lead to complexities and hard-to-debug code. To avoid that the developer has to maintain good naming techniques

say

count = 1 instead of a = 1 . It makes code readable

Reusing a variable names: There is no efficiency gain when reusing names: the assignments will have to create new objects anyway.

It may be a good discipline to avoid assigning to a variable more than once, and it helps in grasping the concept of mutable and immutable types.

### Mutable and immutable types

Mutable types are those that allow in-place modification of the content. Typical mutables are lists and dictionaries. Immutable types provide no method for changing their content.

Mutable Example:
{% highlight ruby %}
list = [1,2,3]
list[0] = 5
{% endhighlight %}
Immutable Example:
{% highlight ruby %}
x = 1
x = x + 1
{% endhighlight %}

Using properly mutable types for things that are mutable in nature and immutable types for things that are fixed in nature helps to clarify the intent of the code.

Strings are immutable.  so, appending each part to the string is inefficient because the entirety of the string is copied on each append. Instead, it is much more efficient to accumulate the parts in a list, which is mutable, and then glue (join) the parts together when the full string is needed

Not suggested way
{% highlight ruby %}
nums = ""
for n in range(20):
    nums += str(n)   # slow and inefficient
print nums
{% endhighlight %}
It Okay but not the best way
{% highlight ruby %}
nums = []
for n in range(20):
    nums.append(str(n))
print "".join(nums)
{% endhighlight %}
It is the best way
{% highlight ruby %}
nums = [str(n) for n in range(20)]
print "".join(nums)
{% endhighlight %}

In the instances where you are creating a new string from a pre-determined number of strings, using the addition operator is actually faster


{% highlight ruby %}
foo = 'foo'
bar = 'bar'

foobar = foo + bar  # This is good
foo += 'ooo'  # This is bad, instead you should do:
foo = ''.join([foo, 'ooo'])


foobar = '%s%s' % (foo, bar) # It is OK
foobar = '{0}{1}'.format(foo, bar) # It is better
foobar = '{foo}{bar}'.format(foo=foo, bar=bar) # It is best
{% endhighlight %}


### Code style

The beauty of python lies in it's readability which is at the heart of the design of the language because code is often read than written

Let us go through some general concepts of python:

```
* Explicit function: the most explicit and straightforward manner is preferred.

* One statement per line: allow one statement for more readability
```

### Function Arguments
Arguments here in Python are passed into functions in 4 ways.

1) Positional Arguments: Let us understand by example. wishFriend(Name,messege) is an example. It shows all the arguments are mandatory and have no default values. A function call can be leveraged here as wishFriend(messege = "Hello", Name = "Alex") but for the sake of readability we follow wishFriend("Alex","Hello").

2)Keyword arguments: These differ from the positional arguments in the fact that all the fields are not mandatory and contains default values. For Example wishFriend(Name,messege,relation="classmate") has relation as a keyword argument. This is very useful where you can just update the function definition and still your existing function calls are not impacted. It's very useful hack(overloading)

3)Arbitary argument list: The arguments are passed into the function as wishFriend(*args). We have to derefernce the arguments that is a list inside our function. It is not suggested to use these unless this is the only option to achieve this. This is because of the fact that it reduces the readability of code.

4)Arbitary keyword argument dictionary: If the function requires an undetermined series of named arguments, it is possible to use the **kwargs construct. In the function body, kwargs will be a dictionary of all the passed named arguments that have not been caught by other keyword arguments in the function signature.

The suggested types of function arguments are 1 or 2 because of readability, easy to change(2nd type), 3 or 4 are used when only there is a proven necessity to use.

### Avoid magical stuff


Pythons provides us many powerful tools but the good coding practice is to write code which passes through code analysis tools with out many hiccups. Some of the code analtic tools include pylint,pyflakes,MyPy, Prospector and Bandit. 
```
* change how objects are created and instantiated
* change how the Python interpreter imports modules
* It is even possible (and recommended if needed) to embed C routines in Python.

```

### Responsible coding

Python has a very different philosophy, very different from highly defensive languages like Java, which give a lot of mechanisms to prevent any misuse, is expressed by the saying: “We are all responsible users”. This means that the developers have to follow some set rules to not break the code while writing the client codes. For example, using _ before and after internal methods is followed as standard and remember that one is free to break the rule.

### Returning values

As similar to any other language we can have multiple return statements for a particular depending upon the usage and conditional logic. There may be some cases where there might be an error or for some reason the function is not able to complete the task. In such cases, it is encouraged to check this at the start of the function. This makes the code readable and the logical part comes only after the error checks and their corresponding returns are written in code. There by reducing the distributed return points from the function.


### Idioms

A programming Idiom is to write te code universally accepted. Some common Python idioms follow:

```
* Unpacking
* Create an ignored variable
* Create a string from a list
* Searching for an item in a collection
* Zen of Python(PEP 20)
* PEP 8
* Auto-Formatting
* Check if a variable equals a constant
* Access a Dictionary Element
```

Some Other Idioms which helps us to play around with list:

`List Comprehensions`: List comprehensions provide a concise way to create lists. i.e a list is created
`Generator Comprehensions` : follows almost the same syntax as list comprehensions but return a generator instead of a list. Here, no list is created by just a generator

Examples:

{% highlight ruby %}
[print(x) for x in sequence] # creates a list

for x in sequence:
    print(x)  # doesn't create a list but just prints
{% endhighlight %}

`Filtering a list` : To filter a list never remove elements from a list instead do:
{% highlight ruby %}
filtered_values = (value for value in sequence if value != x)   # using generator comprehensions
{% endhighlight %}
Modifying the original list can be risky if there are other variables referencing it. But you can use slice assignment if you really want to do that.
{% highlight ruby %}
sequence[::] = [value for value in sequence if value != x]
{% endhighlight %}

`Modifying the values in a list`: assignment never creates a new object. If two or more variables refer to the same list, changing one of them changes them all.
{% highlight ruby %}
a = [3, 4, 5]
b = a                     # a and b refer to the same list object

for i in range(len(a)):
    a[i] += 3             # changing a actually effects b

to avoid this we assign a new list to a made out of a.
a = [i + 3 for i in a]   # now changing a doesn't impact b.

{% endhighlight %}

`Reading from a file`: Using with open function for opening a file. It ensures the close function is called when exited on error
{% highlight ruby %}
f = open('file.txt')
a = f.read()
print a
f.close()

with open('file.txt') as f:
    for line in f:
        print line
{% endhighlight %}

`Line Continuations` : When assigning a string which spans multiple-lines to an variable or some other operation do not use `""" ...."""` because of its fragility: a white space added to the end of the line, after the backslash, will break the code. Instead use parentheses around your elements. Left with an unclosed parenthesis on an end-of-line, the Python interpreter will join the next line until the parentheses are closed. The same behavior holds for curly and square braces.
{% highlight ruby %}
huge_string = """  When assigning a string which spans multiple-lines to an variable or some other operation do not use `""" ...."""` because of its fragility: a white space added to the end of the line, after the backslash, will break the code. Instead use parentheses around your elements. Left with an unclosed parenthesis """

better use

huge_string = ( "When assigning a string which spans multiple-lines to an variable or some other operation do not use `""" ...."""` because" "of its fragility: a white space added to the end of the line, after the backslash, will break the code. Instead use parentheses around " your elements. Left with an unclosed parenthesis")

)
{% endhighlight %}

We have seen too much above best practices in writing code. Now, Let's us see best practices for the documentation

### Project Publication

The basic things needed for the project publication are:
```
* Short Introduction of what can be done using this along-with one or two use cases
* A tutorial which explains your primary use case.
* An API reference listing all publicly available interfaces, parameters, and return values.
* Developer documentation so that the other developers can understand and contribute.
```

### Sphinx

Sphinx converts reStructuredText markup language into a range of output formats including HTML, LaTeX (for printable PDF versions), manual pages, and plain text.

When run, Sphinx will import your code and using Python’s introspection features it will extract all function, method, and class signatures. It will also extract the accompanying docstrings, and compile it all into well structured and easily readable documentation for your project.

### reStructuredText

Most Python documentation is written with reStructuredText. It’s like Markdown, but with all the optional extensions built in.

Now, let us see the code documenation in detail:

### Commenting Sections of Code

Do not use triple-quote strings to comment code. This is not a good practice, because line-oriented command-line tools such as grep will not be aware that the commented code is inactive. It is better to add hashes at the proper indentation level for every commented line.

### DocStrings and taking advantage of them

So, What is Doscstring?

Docstring is something that is present inside the triple quotations of a python code. It helps as a documentation for a particular functionality and its a string, so doc+string=docstring

Some tools use docstrings to embed more-than-documentation behavior, such as unit test logic. Remember, we earlier discussed about Sphinx.

There is another tool called `Doctest` which will read all embedded docstrings that look like input from the Python commandline (prefixed with “>>>”) and run them, checking to see if the output of the command matches the text on the following line.

This allows developers to embed real examples and usage of functions alongside their source code. As a side effect, it also ensures that their code is tested and works.

{% highlight ruby %}
def my_function(a, b):
    """
    >>> my_function(2, 3)
    6
    >>> my_function('a', 3)
    'aaa'
    """
    return a * b
{% endhighlight %}

There is another notation called `Block Comments` which looks similar but usually used to explain what a section of code is doing, or the specifics of an algorithm while docstrings are more intended towards explaining other users of your code or you after some months.


### Writing DocStrings

The docstring should describe the function in a way that is easy to understand. For simple cases like trivial functions and classes embedding these docstrings is quite unnecessary. In larger or more complex projects however, it is often a good idea to give more information about a function, what it does, any exceptions it may raise, what it returns, or relevant details about the parameters.

Refer [NumPy](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html) docstrings if you want to learn more. These are considered to best documented.

There are many other tools like pycco, Ronn, Epydoc, MkDocs but it is adviced to use Sphinx.


Now, Let's move to Test Suite which is as important as writing good code because you need to ship your code to make money!

### Testing Your code

Unit testing is supposed to be done by developer before shipping his code for furthur deployment to avoid un-wanted testing cycles.
Here are some of the basic rules you follow while testing:

```
* 
```

We have some Modules, which helps in writing good test cases.

### unittest
`unittest` is available in standard python libarary so we just need to import and start working. so, Creating test cases is accomplished by subclassing unittest.TestCase.

{% highlight ruby %}
import unittest

def fun(x):
    return x + 1

class MyTest(unittest.TestCase):
    def test(self):
        self.assertEqual(fun(3), 4)
{% endhighlight %}

### Doctest
`Doctest` module searches for pieces of text that look like interactive Python sessions in docstrings and compare the execution results with the expected result present on the nect line of docstring. However, it's primary aim is not to do unit testing.

Example:
{% highlight ruby %}
def square(x):
    """Return the square of x.

    >>> square(2)
    4
    >>> square(-2)
    4
    """

    return x * x

if __name__ == '__main__':
    import doctest
    doctest.testmod()
{%  endhighlight %}

There are many tools available for the test suite. Some of them are [py.test](https://docs.pytest.org/en/latest/) , [hypothesis](https://hypothesis.readthedocs.io/en/latest/) , [tox](https://tox.readthedocs.io/en/latest/) , [mock](https://docs.python.org/3/library/unittest.mock.html) .

I will eloborate them in future blogs but don't count on me.


### Logging

Logging is pretty much important consitituent of the development cycle. You need Logging because I will be a fool if I debug on live-servers.
Formally, We have two purposes of logging:
```
* Diagnostic logging
* Audit Logging
```

It is clear from the names, the reasons why we need logging. Why can't we just print whatever we want on console instead of writing into files and it saves space on servers too? Makes sense? Yes, It makes sense in console applications but on applications we just can't. 

Proper logging helps us with file name, full path, function, and line number of the logging event and the best things is we can disable logs without making changes to code itself.

Furthur, implementations about logging can be extensively found [here](https://docs.python-guide.org/writing/logging/)


### common Mis-conceptions when you start as a beginner

1). Mutable Default Arguments: 

let's say we define:
{% highlight ruby %}
def append_to(element, to=[]):
    to.append(element)
    return to
{%  endhighlight %}
Now, we call
{% highlight ruby %}
my_list = append_to(12)         #function call 1
my_other_list = append_to(13)   #function call 2
{%  endhighlight %}
we expect my_list contains only 12
we expect my_other_list contains only 13

But, my_list contains only 12
my_other_list contains only 12,13

Python’s default arguments are evaluated once when the function is defined, not each time the function is called. In the first function call, we have mutated the list, so it keeps mutated for the second function call.

If we use `to = None` in the functio definition, it would have played the way we expected.


2) Late Binding Closures:

let's say we define: 
{% highlight ruby %}
def create_multipliers():
    return [lambda x : i * x for i in range(5)]
{%  endhighlight %}
and now, we loop like:
{% highlight ruby %}
for multiplier in create_multipliers():
    print(multiplier(2))
{%  endhighlight %}
we expect the results to be 0,2,4,6,8 but the result is 8,8,8,8,8   
explanation:
Python’s closures are late binding. This means that the values of variables used in closures are looked up at the time the inner function is called.

Here, whenever any of the returned functions are called, the value of i is looked up in the surrounding scope at call time. By then, the loop has completed and i is left with its final value of 4.

Solution:
The most general solution is arguably a bit of a hack. Due to Python’s aforementioned behavior concerning evaluating default arguments to functions (see Mutable Default Arguments), you can create a closure that binds immediately to its arguments by using a default arg like so:
{% highlight ruby %}
def create_multipliers():
    return [lambda x, i=i : i * x for i in range(5)]
{%  endhighlight %}

### Disable Bytecode (.pyc) Files

By default, when executing Python code from files, the Python interpreter will automatically write a bytecode version of that file to disk. Theoretically, this behavior is on by default for performance reasons. Without these bytecode files, Python would re-generate the bytecode every time the file is loaded. So, let's get rid of them. but how?

`export PYTHONDONTWRITEBYTECODE=1` environment variable set, Python will no longer write these files to disk, and your development environment will remain nice and clean.






### References: 
Some of the examples and statements are directly taken from [The Hitchhiker's guide to python](https://docs.python-guide.org/)








