---
layout: post
title:  "How is docker changing things?"
date:   2021-01-18 16:23:27 +0530
categories: learn AI
---

### what is Docker?

Let me give the tetbook definition at the start. `Docker` is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly.

Okay, let me explain why Docker is changing things.

As a developer who followed tarditional practices in my 2.5y long carrer, I know how long does it take to make the code after the it is writtem. Shipping, testing and deploying are always an issue if any link in the flow fails the expectations. Docker mainly helps the developer in this, it solves the "it was working on my machine" thing.

We now understand what Docker was trying to solve. There is another concept of `Containers` which forms as a base to the concept of Dockers.

### What is Container?

Just observe , Docker, container and the logo of docker. The theme might have come from the inspiration of Shipping goods through sea. In this analogy once the goods are loaded into the container, what ever happens inside the containers are independent of things happening outside. This is same with our Container as well. Let's get into what a container is.

A container is simply another process on your machine that has been isolated from all other processes on the host machine. That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time. Docker has worked to make these capabilities approachable and easy to use.


### Why Docker is better?

To understand the beauty of something, we better undrstand what is it solving. The traditional way to run applications was to run Virtual machines on Hypervisors. we can run as many VM's as we want on single machines. Look in the figure below:

![Docker-vs-VM](/images/docker-vs-VM.png)

If you have worked on VM's, you will be aware of the fact that they take time to boot and therefore the application spawning takes time. We allocate memory to the VM. So, the applications running of different VM's don't share resources as the walls of their room remain constrained. What if someone says these constraints are not in-place anymore? It would be super interesting, right? That's what docker is all about.

We have our OS(mostly linux) and we have Docker on top of the OS. Now our containers are run on top of the Docker. That's it our applications are up and running now. 

When we compare it with the concept of VM's there is no overhead of OS and more importantly, we have shared resources like CPU, RAM and what not. This makes the concept of Docker and containers very useful and efficient in develment lifecycles.

Let's introduce a concept of image(container image)

### Image(Container Image)

When running a container, it uses an isolated filesystem. This custom filesystem is provided by a container image. Since the image contains the container's filesystem, it must contain everything needed to run an application - all dependencies, configuration, scripts, binaries, etc. The image also contains other configuration for the container, such as environment variables, a default command to run, and other metadata.

Here, I get the shipment containing the container image, I load this image as a conatiner using Docker and I can start using this application. It is that easy to ship our code without having to worry about the "it was working on my machine" idiom. This smoothens the flow of testing and deploying the build once it is approved by the testing team.





