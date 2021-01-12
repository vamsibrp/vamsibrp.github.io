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

### References: 

[The Hitchhiker's guide to python](https://docs.python-guide.org/)








