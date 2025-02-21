---
aliases:
  - containerd
---
# Abstract

**runc** is a command-line tool for creating and running containers in Linux. It is also described as a tool to spawn a new ordinary Linux [[process]] but inside of an isolated environment. This isolation is achieved via Linux namespaces and cgroups.

![Layered Docker architecture: docker (cli) -> dockerd -> containerd -> containerd-shim -> runc](https://iximiuz.com/implementing-container-runtime-shim/docker-containerd-runc-2000-opt.png)



# References