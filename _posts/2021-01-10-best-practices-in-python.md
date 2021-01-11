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






