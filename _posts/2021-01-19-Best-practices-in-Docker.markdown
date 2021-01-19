---
layout : post
title : Best Practices in Docker
date : 2021-01-19 16:23:27 +0530
Categories : Docker
---

### Need for efficient Dockerfile

The main aim of this tutorial is to provide an comprehensive understanding on sections of Dockerfile and the best practices that we can follow. We will consider many things like caching, image sizes, maintainability and many more...

Let's dive deep into it.

Before diving deep let me remind the fact that, every step in the file is a layer which is being stacked on the previous layer.

```
step 1: base image

step 2: do something.

step 3: do something.
```
In the above example, on moving through the step 2, a layer gets stacked on the base image creating an intermediate image(IMG-2 say). On going through the step 3, a layer gets stacked on IMG-2 and creates and intermediate image(IMG-3 say). 

`docker image history {name}`command, you can see the command that was used to create each layer within an image. `--no-trunc` flag, you'll get the full output in case of truncated output.

Now, we are aware of how Layering works in creating an image. So let's jump into the best practices.

### Base image selection

* To start with, use official images whenever possible because all the installation steps are done and best practices are applied. 
* Always don't look for the latest version but look for the last stable image. Use more specific tags for your base images to prevent build failures.
* Look for the images which are smaller in size. This helps you in making your final Image sleak and more-preferred.

The next thing we will discuss is caching. Yes, we have caching even increating an image.

### Caching related practices

In development cycles, we keep on making changes to our code which ulimately has an impact on the image that we ship. It is wise, to leverage the concept of caching to prevent un-necessary repetetive build steps.

Let's assume the below given statements are steps in a Dockerfile 
```
step 1: base image

step 2: do something.

step 3: do something.

step 4: do something.
```

When the Dockerfile is run for the very first time, all the steps are cached. 

Let's assume a code change was made in code of step 3 and we are building the image. 

In step 1 and step 2, there are no changes and therefore no need to build them. So they are taken from the cache.

Since the change was in step 3, all the steps that come thereafter have to be built instead of taking from the cache and cached after build at each step succeeds.

Now we have idea of how caching works in building an image. So, Let's look into some best practices here.

* Ordering the build steps matters too much because when a step’s cache is invalidated by changing files or modifying lines in the Dockerfile, subsequent steps of their cache will break. 
* Identify cacheable units such as apt-get update & install. Let me explain this with an example.

Wrong practice:
{% highlight ruby %}
Step n: sudo apt-get update

step n+1: sudo apt-get install {Package1}

step n+2: sudo apt-get install {Package2}
{% endhighlight %}

you always want to update the index and install packages in the same RUN: they form together one cacheable unit. Otherwise you risk installing outdated packages. At the same time clubbing step n+1 and step n+2 bust the caching at that step. So have to be wise while clubbing different run commands.

### Reducing Image-Size

We all prefer slim images because they are easy to ship and easy deployment.

* removing unnecessary dependencies contribute a lot in reducing the image size. Apt has the `–no-install-recommends` flag which ensures that dependencies that were not actually needed are not installed. If they are needed, add them explicitly.

* Removing package Manager cache: Package managers maintain their own cache which may end up in the image. One way to deal with it is to remove the cache in the same RUN instruction that installed packages. Removing it in another RUN instruction would not reduce the image size.

We can furthur reduce the image size by maintaining multi-stage builds. 

### Multi-stage builds

Although this may not be really useful during development time, we cover it quickly as it is interesting for shipping the containerized Python application once development is done. 

What we intend in using multi-stage builds is to strip the final application image of all unnecessary files and software packages and to deliver only the files needed to run our Python code.  A quick example of a multi-stage Dockerfile is the following:
{% highlight ruby %}
# first stage
FROM python:3.8 AS builder
COPY requirements.txt .

# install dependencies to the local user directory (eg. /root/.local)
RUN pip install --user -r requirements.txt

# second unnamed stage
FROM python:3.8-slim
WORKDIR /code

# copy only the dependencies installation from the 1st stage image
COPY --from=builder /root/.local/bin /root/.local
COPY ./src .

# update PATH environment variable
ENV PATH=/root/.local:$PATH

CMD [ "python", "./server.py" ]
{% endhighlight %}

Note that we have a two stage build where we name only the first one as builder. We name a stage by adding an AS <NAME> to the FROM instruction and we use this name in the COPY instruction where we want to copy only the necessary files to the final image.





