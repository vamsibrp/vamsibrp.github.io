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
{% highlight ruby %}
`from modu import *`
x = sqrt(4)
{% endhighlight %}

Approach B:
% highlight ruby %}
from modu import sqrt
x = sqrt(4)
{% endhighlight %}

Approach C:
% highlight ruby %}
import modu
x = modu.sqrt(4)
{% endhighlight %}

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








