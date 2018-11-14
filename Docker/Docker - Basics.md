## Docker Basics

You can have a lot of issues developing applications that use a lot of different components. You can have issues with OS compatibility, services and libraries. Your application can change overtime aswell, and everytime something changes compatibility has to be checked. This is known as the Matrix from Hell. It's also very complicated to set up the development environment for new developers. 
It can make developing applications very difficult.
With Docker you can run each service in it's own container with it's own libraries and dependencies. 

Containers are completely isolated environments. They can have their own processes and services, their own networks and their own mounts. 

Operating systems often consist of just two things. An OS kernel, and a set of software. The kernel is responsible for interacting with the underlying hardware. You have a common linux kernel across all operating systems and a set of custom software that differentiates operating system.

### Sharing the Kernel
If, for example, you have a system with Ubuntu OS installed on a linux kernel. Docker can run any flavour of os on top of it as long as they are based on the same kernel. If the underlying operating system is Ubuntu Docker can run a container based on another distrubution. Each Docker container only has the additional software that makes the software different, and utilises the underlying kernel of docker host which works with all the Os's above. 
You won't be able to run a Windows based container on a Docker host with linux OS on it. Docker isn't designed to run different kernels on hardware. Dockers main purpose is to containerize applications, ship them and then run them. 

### Containers vs Virtual Machines
With containers you have the Hardware infrastructure, then the os, then Docker which creates it's containers. The containers run with libraries and dependencies.
With virtual machines Docker is replaced by a Hypervisor. The difference is that each VM has it's own OS installed which uses up a large amount of processes, take up a lot of size and are slow to boot up. Docker has less isolation as more resources are shared between containers like the kernel. Vms have complete isolation from each other. 

### How does it work?
Most organisations have their products containerised and made publicly available in Docker Hub. You can find images of OS's, databases and other services. `docker run ` allows you to run an instance of a service in the docker host. 

A docker image is a package or a template. It is used to create one or more containers. 
A container is a running instance of an image. 