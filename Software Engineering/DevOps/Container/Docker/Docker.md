---
tags:
  - docker
  - container
---
---
# Introduction

Docker is an open platform for developing, shipping and running applications. It enables the separation of applications from infrastructure to deliver software quickly. Docker provides the ability to package and run an application in a loosely isolated environment called a container.

# Architecture
Docker use Client-Server Architecture.
Clinet & Server communicate using a REST API over UNIX socket or a network interface
Components
- Docker Client: -> Docker CLI
- Docker Daemon: -> Server. The core engine that manages Docker operations. It runs on the host operating system and manages Docker services and objects like images, containers, networks, and volumes... The daemon listens for Docker API requests and processes commands to manage containers and images. It can also communicate with other daemons in a distributed system to facilitate orchestration tasks across multiple hosts
- Docker Host: -> Máy chạy docker daemon
- Docker Registry

*Docker Daemon* use [[Container Runtime|container runtime]] to execute containers.

# References