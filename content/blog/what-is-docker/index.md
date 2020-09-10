---
title: What is Docker? [DevOps]
description: What is Docker and what is the hype about?
date: "2020-09-10T23:46:37.121Z"
---

The following article attempts to explain what came before **Docker**, what **Containers** are and why it's a good idea to use **Docker** in your development and production environment. It's not a tutorial on how to use Docker. Perhaps I will create one in the future. In the meantime, you can refer to the **Docker** [website](https://www.docker.com/) for more information as well as tutorials.

## Pre Containers

Before Docker, developers used **VMs (Virtual Machines)** to deploy, ship and run software. You can think of **VM** as a computer within a computer. Within a **VM**, a user has access to the same applications, user interfaces and settings as if it were your own computer. Therefore, it is possbible to run what appears to be multiple computers on a single machine.

The operating systems (OS) and their applications share hardware resources from a single host server, or from a pool of host servers. Each VM requires its own underlying OS, and the hardware is virtualized. A hypervisor, or a virtual machine monitor, is software, firmware, or hardware that creates and runs VMs. It sits between the hardware and the virtual machine and is necessary to virtualize the server.

### Downside to VMs

VMs can take up a lot of system resources. Each VM runs not just a full copy of an **OS**, but a virtual copy of all the hardware that the operating system needs to run. This quickly adds up to a lot of RAM and CPU cycles. That’s still economical compared to running separate actual computers, but for some applications it can be overkill, which led to the development of containers.

### Advent of Containers

Instead of virtualizing the underlying computer like a virtual machine (VM), Containers virtualize only the **OS**.

To be clear, **Docker** did not being the **Container** revolution. It dated back to at least the year 2000 with **FreeBSD Jails**, **OpenVZ**, **LXC (Linux Containers)** and **mctfy (Let Me Contain That For You)**. And **Docker** is built on top of LXC. However, what Docker has done is that it brought several new things to the table that the earlier technologies didn't. The first is it's made containers easier and safer to deploy and use than previous approaches. In addition, because Docker's partnering with the other container powers, including Canonical, Google, Red Hat, and Parallels, on its key open-source component libcontainer, it's brought much-needed standardization to containers.

### What are Containers?

Containers sit on top of a physical server and its host OS — typically Linux or Windows. Each container shares the host OS kernel and, usually, the binaries and libraries, too. Shared components are read-only. Sharing OS resources such as libraries significantly reduces the need to reproduce the operating system code, and means that a server can run multiple workloads with a single operating system installation. Containers are thus exceptionally light — they are only megabytes in size and take just seconds to start. Compared to containers, VMs take minutes to run and are an order of magnitude larger than an equivalent container.

In contrast to VMs, all that a Docker Container requires is an OS. In layman terms, it means you can put two to three times as many as applications on a single server with containers than you can with a VM. Wow. In addition, with containers you can create a portable, consistent operating environment for development, testing, and deployment.

### Key benefits of Docker

1. Portability
With Docker, you basically build an **Image** encapsulating all the necessary dependencies  With the Docker **Image**, you can easily port it over to any machine (Linux or Windows), and run it without worrying it will break. The **Image** will run exactly the way you expect on the machine that you had tested on.

2. Performance
Docker containers do not contain an operating system (whereas virtual machines do). Therefore, it has a much smaller footprints than virtual machines.

3. Scalability
You can quickly create new containers on demand. When using multiple containers you can take advantage of a range of container management options. For example, with **Kubernetes**, you can easily create new Docker Containers and delegate the management of the Containers to **Kubernetes**.

4. Faster and Safer CI/CD
The portability and performance benefits offered by containers can help you make your development process more agile and responsive. On top of that, it provides safety knowing that the **Image** you deliver will work exactly the way it was tested in the test environment.

## Conclusion

To summarize, **Docker** is a fantastic option for developers and has become, perhaps, the de-facto choice for most. There's nothing not to like about it, and if your organisation allows for it, it's a game-changer. The hype is real, and is here to stay.