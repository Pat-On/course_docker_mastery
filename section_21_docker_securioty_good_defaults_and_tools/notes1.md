# 160 Section Intro: Top 10 Security Steps for Docker

List: (source: https://github.com/BretFisher/ama/discussions/150)

# 161 Docker Namespaces and Cgroups

## Linux Utilities to Lock Down Docker

Kernel namespaces (from docker documentation)
Docker containers are very similar to LXC containers, and they have similar security features. When you start a container with docker run, behind the scenes Docker creates a set of namespaces and control groups for the container.

Namespaces provide the first and most straightforward form of isolation: processes running within a container cannot see, and even less affect, processes running in another container, or in the host system.

Each container also gets its own network stack, meaning that a container doesn’t get privileged access to the sockets or interfaces of another container. Of course, if the host system is setup accordingly, containers can interact with each other through their respective network interfaces — just like they can interact with external hosts. When you specify public ports for your containers or use links then IP traffic is allowed between containers. They can ping each other, send/receive UDP packets, and establish TCP connections, but that can be restricted if necessary. From a network architecture point of view, all containers on a given Docker host are sitting on bridge interfaces. This means that they are just like physical machines connected through a common Ethernet switch; no more, no less.

How mature is the code providing kernel namespaces and private networking? Kernel namespaces were introduced between kernel version 2.6.15 and 2.6.26. This means that since July 2008 (date of the 2.6.26 release ), namespace code has been exercised and scrutinized on a large number of production systems. And there is more: the design and inspiration for the namespaces code are even older. Namespaces are actually an effort to reimplement the features of OpenVZ in such a way that they could be merged within the mainstream kernel. And OpenVZ was initially released in 2005, so both the design and the implementation are pretty mature.

Control groups (from docker documentation)
Control Groups are another key component of Linux Containers. They implement resource accounting and limiting. They provide many useful metrics, but they also help ensure that each container gets its fair share of memory, CPU, disk I/O; and, more importantly, that a single container cannot bring the system down by exhausting one of those resources.

So while they do not play a role in preventing one container from accessing or affecting the data and processes of another container, they are essential to fend off some denial-of-service attacks. They are particularly important on multi-tenant platforms, like public and private PaaS, to guarantee a consistent uptime (and performance) even when some applications start to misbehave.

Control Groups have been around for a while as well: the code was started in 2006, and initially merged in kernel 2.6.24.

Links:

- https://docs.docker.com/engine/security/
- https://sysdig.com/blog/20-docker-security-tools/

---

# 162 Docker Engine's Out-Of-The-Box Security Features

Links:

- https://docs.docker.com/engine/security/seccomp/
- https://docs.docker.com/engine/security/apparmor/

# 163 Docker Bench, The Host Configuration

Links:

- https://github.com/docker/docker-bench-security - INTERESTING
- https://www.cisecurity.org/benchmark/docker

# 164 Using USER in Dockerfiles to Avoid Running as Root

Link: https://docs.docker.com/engine/reference/builder/#user

# 165 Docker User Namespace for Extra Host Security

Links:

- https://integratedcode.us/2015/10/13/user-namespaces-have-arrived-in-docker/
- https://docs.docker.com/engine/security/userns-remap/

# 166 Code Repo and Image Scanning for CVE's

links:

- https://cve.mitre.org/
- https://snyk.io/
- https://www.paloaltonetworks.com/prisma/cloud
- https://github.com/aquasecurity/microscanner
- https://github.com/aquasecurity/trivy

# 167 Sysdig Falco, Content Trust and Custom Seccomp and AppArmor Profiles

- https://sysdig.com/opensource/falco/
- https://docs.docker.com/engine/security/trust/
- https://docs.docker.com/engine/security/seccomp/
- https://docs.docker.com/engine/security/apparmor/

# 168 Docker Rootless Mode

- https://github.com/docker/engine/blob/v19.03.0-rc3/docs/rootless.md
- https://www.docker.com/
- https://www.youtube.com/watch?v=Qq78zfXUq18
- https://get.docker.com/rootless

# 169 The Security top 10 Differences for Windows Containers

- Different system so you need to find a new tools and create almost new list :>

# 170 What are Distroless images

- https://github.com/GoogleContainerTools/distroless
