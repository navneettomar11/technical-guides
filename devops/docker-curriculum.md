# Virtual Machine
In computing, a virtual machine (VM) is an emulation of a computer system. Virtual machines are bashed on computer architectures and provide functionality of a physical computer. Their implementations may involved specialized hardware, software or a combination.

A **Virtual Machine** (VM) is a compute resource that uses software instead of a physical computer to run programs and deploy apps. One or more virtual "guest" machines run on a physical "host" machine. Each virtual machine runs its own operating systems and functions separately from the other VMs, even when they are running on the same host. This means that, for example, a virtual MacOS virtual machine can run on a physical PC.

Virtual machine technology is used for many uses case across on-premises and cloud environments. More recently, public cloud services are using  virtual machines to provide virtual application resources to multiple users at once, for even more cost efficient and flexible compute.

## What are virtual machines used for?
Virtual machines (VMs) allow a business to run an operating system that behaves like a completely separate computer in an app window on a desktop. VMs may be deployed to accommodate different level of processing power needs to run software that required a different operating system, or to test applications in a safe, sandboxed environment.

Virtual machines have historically been used for server virtualization which enables IT teams to consolidate their computing resources and improve efficiency. Additionally, virtual machines can perform specific task considered too risky to carry out in a host environment, such as accessing virus-infected data or testing operating systems. Since the virtual machine is separated from the rest of the system, the software inside the virtual machine cannot tamper with the host computer.

## How do virtual machines work?
The virtual machines run as a process in an application window, similar to any other application, on the operating syste, of the physical machine. Key files that make up a virtual machine include a log file, NVRAM setting file, virtual disk file and configuration file.

## Advantages of Virtual machines
Virtual machines are easy to manage and maintain and they offer several adavantages over physical machines:
- VMs can run multiple operating system environments on a single physical computer, saving physical space, time and management costs.
- Virtual Machines support legacy applications, reducing the cost of migrating to a new operating system. For example, a Linux virtual machine running a distribution of Linux as the guest operating system can exist on a hist server that is running a non-Linux operating system such as Windows.
- VMs can also provide integrated diaster recovery and application provsioning options.

## Disadvantages of Virtual machines
While virtual machines have several advantages over physical machines, there are also some potential disadvantages:
- Running multiple virtual machines on one physical machine can result in unstable performance if infrastructure requirment are not met.
- Virtual machines are less efficient and run slower than a full physical computer. Most enterprises use a combination of phyiscal and virtual infrastructure to balance the corresponding advantages and disadvantages.

## The two types of virtual machines
User can chosses from two different types of virtual machines - process VMs and system VMs:
- **A process virtual machine** allows a single process to run as a application on a host machine, providing a platform-independent programming environment by masking the information of the underlying hardware or operating system. An example of a process VM is the Java Virtual Machine which enables any operating system to run Java applications as if they were native to that system.
- **A system virtual machine** is fully virtualized to substitute for phyiscal machine. A system platform supports the sharing of a host computer's physical resources between multiple virtual machines, each running its own copy of the operating system. This virtualization process relies on a hypervisor, which can run on bare hardware, such as VMWare ESXi, or on top of an operating system.

## What are 5 types of virtualization?
ALl the components of a traditional data center or IT infrastructure can be virtualized today, with various specific types of virtualization:
- **Hardware virtualization**: When virtualizing hardware, virtual versions of computers and operating systems (VMs) are created and consolidated into a single, primary, physical server. A hypervisor communicates directly with a physical server's disk space and CPU to manage VMs. Hardware virtualization, which is also known as server virtualization, allow hardware resources to be utilized more efficiency and for one machine to simultanesouly run different operating system.
- **Software virtualization**: Software virtualization creates a computer system complete with hardware that allows one or more guest operating system to run on a physical host machine. For example, Android OS can run on a host machine that is natively using a Microsoft Window OS utilizing the same hardware as the host machine does. Additionally, applications can be virtualized and delivered from a server to  an end user's, device, such as a laptop or smartphone. This allows employee to access centrally hosted applications when working remotely.
- **Storage virtualization**: Storage can be virtualized by consolidating multiple physical storage devices to appear as a single storage device. Benefits include increased performance and speed load balancing and reduced costs. Storage virtualization also helps with disaster recovery planning as virtual storage data can be duplicated and quickly transfered to another location, reducing downtime.
- **Network virtualization**: Multiple sub-networks can be created on the same physical network by combining equipment into a single, software-based virtual network resource. Network virtualization also divides available bandwidth into multiple, independent channels, each of which can be assigned to servers and devices in real time Advantages includes increased reliability, network speed, security and better monitoring of data usage. Network virtualization can be a good choices for companies with a high volumne of users who need access at all times.
- **Desktop virtualization**: This common type of virtualization separates the desktop environment from the physical devices and stores on a remote server allowing user to access their desktops from anywhere on any device. In addition to easy accessibility, benefits of virtual desktops include better data security, cost saving on software licesnes and updates and ease of management.


# What is a hypervisor ?
A **hypervisor** also known as a virtual monitor or VMM is software that creates and run virtual machines (VMs). A hypervisor allows one host computer to support multiple guest VMs by virtually sharing its resources, such as memory and processing.

## Why use a hypervisor?
Hypervisors make it possible to use more of a system available resources and provide greater IT mobility since the guest VMs are independent of the host hardware. This mean they can be easily move between different servers. Because multiple virtual machines can run off of one physical server with a hypervisor a hypervisor reduces:
- space
- engery
- Maintainance requirments


# Docker Overview
Wikipedia defines Docker as
> an open-source project that automates the deployment of software applications inside containers by providing an additional layer of abstraction and automation OS-level virtualization on Linux.

Docker is an open platform for developing, shipping and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker's methodologies for shipping, testing and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.

# The Docker platform
Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight because they don't need the extra load of a hypervisor, but run directly within the host machine's kernel. This means you can run more containers on a given hardware combination than if you were using virtual machines. You can even run Docker containers within host machines that are actually virtual machines!

Docker provides tooling and a platform to manage the lifecycle of your containers:
- Develop your application and its supporting components using containers.
- The container becomes the unit for distributing and testing your application.
- When you're ready, deploy your application into your production environment, as a container or an orchesreated service. This works the same whether your production environment is a local data center, a cloud provider, or a hybrid of the two.

# Docker Engine
Docker Engine is a client-server application with these major components:
- A server which is a type of long-running program called a daemon process(the `dockerd` command).
- A REST API which specifies interfaces that programs can use to talk to the daemon and instruct it what to do.
- A command line interface (CLI) client (the `docker` command).

![Docker Engine](https://docs.docker.com/engine/images/engine-components-flow.png)

The CLI uses the Docker REST API to control or interact with the Docker daemon through scripting or direct CLI commands. Many other Docker applications use the underlying API and CLI.

# What can I use Docker for?
**Fast, consistent delivery of your applications**.
Docker streamlines the development lifecycle by allowing developers to work in standardized environments using local containers which provide your applications and services. Containers are great for continous integration and continous delivery (CI/CD) workflows.

Consider the following example scenario:
- Your developer write code locally and share their work their collegues using Docker container.
- They use Docker to push their applications into a test environment and execute automated and manual tests.
- When developers find bugs, they can fix them in the development environment and reploy them to the test environment for testing and validation.
- When testing is complete, getting the fix to the customer is as simple as pushing the updated images to the production environment.

**Responsive deployment and scaling**
Docker's container-based platform allows for highly portable workloads. Docker containers can run on a developer's local laptop, on physical virtual machines in a data center, on cloud providers, or in a mixture of environments.

Docker's protability and lightweight nature also make it easy to dynamically manage workloads, scaling up or tearing down applications and services as business needs dictate, in near real time.

**Running more workloads on the same hardware**
Docker is lightweight and fast. It provides a viable, cost-effective alternative to hypervisor-based virtual machines. so you can use more of your compute capacity to achieve your business goals. Docker is prefect for high density environments and for small and medium deployments where you need to do more with fewer resources.

# Docker architecture
Docker uses a client-server architecture. The Docker clients talks to the Docker daemon, which does the heavy lifting of building, running and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or network interface.

![Docker architecture](https://docs.docker.com/engine/images/architecture.svg)

## The Docker daemon
The Docker daemon(dockerd) listens for Docker API requests and manages Docker objects such as images, containers, networks and volumnes. A daemon can also communicate with other daemons to manage Docker services.

## The Docker client
The Docker client(docker) is the primary way that many Docker users interact with Docker. When you use commands such as `docker run`, the client sends these commands to `dockered`, which carries them out. The `docker` command uses the Docker API. The Docker client can communicate with more than one daemon.

## Docker registries
A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use, and Docker is configured to look for images on Docker Hub by default. You can even run your own private registry.

When you use the `docker pull` or `docker run` commands, the required images are pulled from your configured registry. When you use the `docker push` command, your image is pushed to your configured registry.

## Docker objects
When use Docker, you are creating and using images, containers, networks, volumnes, plugins and other objects. This section is a brief overview of some of those objects.
### Images
A image is a ready-only template with instructions for creating a Docker container. Often, an image is based on another images, with some additional customization. For example, you may build an image which is based on the `ubuntu` image, but installs the Apache web server and your application, as well as the configuration details needed to make your application run.

You might create your own images or you might only use those created by other and published in a registry. To build your own image, you create a `DockerFile` with a simple syntax for defining the steps needed to create the image and run it. Each instruction in a DockerFile creates a layer in the image. When you change the DockerFile and rebuild the image, only those layers which changed are rebuilt. This is part of what makes images so lightweight, small and fast, when compared to other virtualization technologies.

### Containers
A container is a runnable instance of an image. You can create. start, stop, move or delete a container using the Docker API or CLI. You can connect a container to one or more networks, attach storage to it, or even create a new image based on its current state.

By default, a container is relatively well isolated from other containers and its host machine. You can control how isolated a container's network, storage, or other underlying subsystems are from other containers or from the host machine.

A container is defined by its images as well as any configuration options you provide to it when you create or start it. When a container is removed any changes to its state are not stored in persistent storage disappear.

**Example `docker run` command**
The following command runs an `ubuntu` container, attaches interactively to your local command-line session, and run `/bin/bash`

```bash
docker run -i -t ubuntu /bin/bash
```
When you run this command, the following happens(assuming you are using the default registry configuration):
1. If you do not have the `ubuntu` image locally, Docker pulls it from your configured registry, as though you have run `docker pull ubuntu` manually.
2. Docker creates a new container, as though you had run a `docker container create` commnand manually.
3. Docker allocates a read-write filesystem to the container, as its final layer. This allows a running container to create or modify files and directories in  its local filesystem.
4. Docker creates a network interface to connect the container to the default network, since you did not specify any networking options. This includes assigning and IP address to the container. By default, containers can cannot to external networks using the host machine network connection.
5. Docker starts the container and executes `/bin/bash`. Because the container is running interactively and attached to your terminal(due to the `-i` and `-t` flags, you can provide input using your keyboard while the output is logged to your terminal.
6. When you type `exit` to terminated the `/bin/bash` command, the container stops but is not removed. You can start it again or remove it.

The industry standard today is to use Virtual Machines (VMs) to run software applications. VMs run application inside a guest OS, which runs in virtual hardware power by the server's host OS.

VMs are great at providing full process isolation for applications: there are very few ways a problem in the host operating system can affect the software running in the guest operating system, and vice-versa. But this isolation comes at great cost - the computation overhead spent virtualizing hardware for a guest OS to use is substantial.

Containers take a different approach: by leveraging the low-level mechanics of the host operating system, containers provide most of the isolation of virtual machines at a fraction of the computing power.

#### Containers vs Virtual Machine
Like virtual machines, container technology such as Kubernetes is similar in the sense of running isolated applications on a single platform. While virtual machine virtualize the hardware layer to create a "computer" containers package up just a single app along with its dependencies. Virtual machine are often managed by hypervisor, where container system provide shared operating system services from the underlying and isoslate the application using virtual-memory hardware.

A key benefit of containers is that they have less overhead compared to virtual machines. Containers include only the binaries, libraries and other required dependenciesm and the application. Containers that are on the same host share the same operating system kernel, making containers much smaller than virtual machines. As a result, container boot faster, maximize server, resources and make delivering application easier. Containers have become popular to use case such as web applications. DevOps testing, microservices and maximizing the number of apps that can be deployed per server.

Virtual machines are larger and slower to boot than containers. They are logically isolated from one another, with their own operating system kernel and offer the benefits of a completely separate operating system. Virtual machines are best for running multiple applications together, monolithic applications, isolation between apps, and for legacy apps running on older operating systems Containers and virtual machines may also be used together.

#### Why use containers ?
Containers offer a logical packaging mechanism in which applications can be abstracted from the environment in which thet actually run. This decoupling allows container-based applications to  be deployed easily and consistently, regardless of whether the target environment is a private data center, the public cloud or even a developer personal laptop. This gives developers the ability to create predictable environments that are isolated from the rest of the applications and can be run anywhere.

From an operations standpoint, apart from portability containers also give more granular control over resources giving your infrastructure improved efficiency which can result in better utilization of your computer resources.

Due to these benefits, containers(& Docker) have seen widespread adoption. Companies like Google, Facebook, Netflix and Salesforce leverage containers to make large engineering teams more productive and to improve utilization of computer resources. In fact, Google credited containers for eliminating the need for an entire data center.

### Services
Services allows you to scale containers across multiple Docker daemons which all work together as a swarm with multiple managers and workers. Each member of swarm is a Docker daemon, and all the daemons communicate using the Docker API. A service allows you to defined the desired state, such as the number of replicas of the service that must be available at any given time. By default, the service is load-balanced across all worker nodes. To the consumer, the Docker service appears to be a single application. Docker Engine supports swarm mode in Docker 1.12 and higher.

# The underlying technology
Docker is written in the Go programming language and takes advantage of several features of the Linux kernel to deliver its functionality.

## Namespaces
Docker uses a technology called `namespaces` to provide the isolated workspace called the container. When you run container, Docker creates a set of namespaces for that container.

These namespaces provide a layer of isolation. Each aspect of a container runs in a separate namespace and its access is limited to that namespace.

Docker Engine uses namespaces such as the following on Linux:
- The `pid` namespace: Process isolation(PID: Process ID)
- The `net` namespace: Meaning network interfaces (NET: Networking)
- The `ipc` namespace: Managing access to IPC resources (IPC: Interprocess Communication)
- The `mnt` namespace: Managing filesystem mount points(MNT: Mount)
- The `uts` namespace: Isolating kernel and version identifiers.(UTS: Unix Timesharing System)

## Control groups
Docker Engine on Linux also relies on another technology called control groups (`cgroups`). A cgroup limits an application to a specific set of resources. Control groups allow Docker Engine to share available hardware resource to container and optionally enforce limits and constraints. For example, you can limit the memory available to a specific container.

## Union file systems
Union file systems, or UnionFS, are file systems that operate by creating layers, making them very lightweight and fast. Docker Engine ues UnionFS to provide the building blocks for containers. Docker Engine can uses multiple UnionFS variants, includung AUFS, btrfs, vfs and DeviceMapper.

## Container Format
Docker Engine combines the namespaces, control groups and UnionFS into a wrapper called a container format. The default container format is `libcontainer`. In future Docker may support other container formats by intergrating with technologies such as BSD Jails or Solaris Zones.


**Source**: 
- [https://www.vmware.com/topics/glossary/content/virtual-machine](https://www.vmware.com/topics/glossary/content/virtual-machine)
- [https://docker-curriculum.com/](https://docker-curriculum.com/)
- [https://docs.docker.com/get-started/overview/](https://docs.docker.com/get-started/overview/)