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


### Dockerfile

A Dockerfile is simply a text-based script of instructions that is used to create a container image. This file is heart of the concept.

### How to create an image

Make sure we have Dockerfile inside the root directory of the project. Once we have Dockerfile, build the container image using the `docker build` command.

{% highlight ruby %}
docker build -t {name} .
{% endhighlight %}

The `-t` here tags the image created now to the `name` we name the image.

The `.` here at the end of the command suggests that the Docker should look for `Dockerfile` in the current directory

Now, we have our image ready to ship to anyone anywhere

### Start our container on docker

we will use `docker run` command for this purpose

{% highlight ruby %}
docker run -dp {host-port}:{container-port} {name}
{% endhighlight %}
Here ,the flag d means detached and running in back-ground and flag p means creating a mapping between the host's port to the container's port 

Example: 

Let me pull Redis from Docker hub. Refer [this](https://hub.docker.com/_/redis)

`docker pull redis` get the redis image from docker-hub

{% highlight ruby %}
(base)  vamsibrp.github.io % docker run -dp 6379:6379 redis
e7c67adb991e1300bb86.....0d132646a2fddf073131a9eaec71c10ff1599c 
{% endhighlight %}

This created a redis container on 6379 port. If you are on mac or Windows you can see an instance getting initiated with a random name in Docker Desktop app.

Once we create and run the container on a docker, we face an issue when we made some changes to some part of code. Let's look a bit into this

### Updating the Application

let's assume we have made changes to our application and try to re-start the container using `docker run`.

Now we might encounter an issue :
{% highlight ruby %}
docker: Error response from daemon: driver failed programming external connectivity on endpoint laughing_burnell 
(bb242b2ca4d67eba76e79474fb36bb5125708ebdabd7f45c8eaf16caaabde9dd): Bind for 0.0.0.0:6379 failed: port is already allocated.
{% endhighlight %}

Remember, our container on port 6379 is already running. It our responsibility to stop and remove the running container from the docker. Note here that stoping and removing both need to be done as a best practice.

we work with the container id's to identify them. So `docker ps` command gives the id's of container.

{% highlight ruby %}
docker stop {container-ID}   # stops the container with given ID

docker rm {container-ID}     # removes the conatiner with given ID
{% endhighlight %}

Now, we can follow the process of building the image and running the image as explained in the previous steps.

### Shipping APP to Docker hub

This is important to learn because we as developers should help each others with our contributions to docker-hub. So we should have our hands-on shipping to Docker-hub

```
* Create a Docker-Hub Repository on the web-site
* Now, we need to push our local-image to Docker-Hub repository.

```

You can DIY the first step.

Coming to the second step, We need to push to our repository on DockerHub

Let's tag our local image to the repository on docker-hub

{% highlight ruby %}
docker tag {local-image} {username}/{image-name}

docker push {username}/{image-name}
{% endhighlight %}

You can verify the changes on your docker-hub repository


### Multi-Container APPS

Till Now, we were only working with single conatiner APP. In ideal case it will not be the case. We will be needing two or more applications to talk to each other.

Idea 1: Why not have all the applications in one container?
By doing so, we are maintaining the isolated form of the application but losing out on maintainability.

Idea 2: We can have seperate isolated containers for different applications
This looks good but we stated that these containers are isolated. We need a network to connect these containers. That is where we arrive at the concept of Container Networking. If two containers are on the same network, they can talk to each other. If they aren't, they can't.

How to create a network?
`docker network create {app-name}`

How to run Images on network?
`docker run -{flags} --network {app-name} image1`

In the above command, flags(like d,p,i,t) serve the required purpose

we can add 2 or more containers to a declared container

There is another Docker tool to work with multi-container applications. 

### Docker Compose

Docker Compose is a tool that was developed to help define and share multi-container applications. With Compose, we can create a YAML file to define the services and with a single command, can spin everything up or tear it all down.


The big advantage of using Compose is you can define your application stack in a file, keep it at the root of your project repo (it's now version controlled), and easily enable someone else to contribute to your project. Someone would only need to clone your repo and start the compose app. In fact, you might see quite a few projects on GitHub/GitLab doing exactly this now.

Refer [Github](https://github.com/docker/compose) for more detailed explaination on Compose.

A sample Compose file looks something like this :
{% highlight ruby %}
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
  redis:
    image: redis
{% endhighlight %}

Compose is all about writing `docker-compose.yml` file. Once we have thsi file ready , we can easily start-up

Most importantly, make sure you have nothing running on the ports we are about run our containers on(i.e stop and remove other running copies)

Start up the application stack using the docker-compose up command. We'll add the -d flag to run everything in the background.
{% highlight ruby %}
docker-compose up -d   # start the container stack

docker-compose down    # simply tear-down or stop the container stack.
{% endhighlight %}

This ends the basic tutorial on Docker. It is short but a crisp idea of Docker and how it can impact development lifecycles.

### Up-Next

I'll come up with a working example and with the best-practices to be followed while writing Dockerfile

### References:

[Github](https://github.com/docker/getting-started) repository for docker.







