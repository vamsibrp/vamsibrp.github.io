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
