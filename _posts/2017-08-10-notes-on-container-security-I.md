---
layout: post
title: notes on container security I
---

_This post is a set of notes from Jess Frazelle's talk 'Benefits of isolation provided by containers' delivered at O'Reilly Security 2016 Conference in New York. Notes are abbreviated in some places and in places where I was not familiar with concepts from the talk, I've added some of my own clarifications/definitions. All mistakes are mine._

### How do containers help security?
- they do not prevent application compromise, but can limit the damage

- the world an attacker sees inside a container is very different than if she would be looking at an app running without a container

### What is a container?

- a group of Linux namespaces and control groups applied to a process

### What are control groups (cgroups) ?

Cgroups limit what a process can use. There various kinds of cgroups: for example, the memory cgroup limits what physical or kernel memory. Another example, the blkio group limits various I/O operations. 
cgroups are controlled by files

### What are namespaces ?

Namespaces limit what a process can see. 
They are files located in /proc/{pid}/ns

### Docker and defaults

- by default Docker blocks writing to /proc/{num} and /proc{sys}, mounting and writing to sys

- LSM (Linux Security Modules ) - a framework that allows the Linux kernel to support a variety of security modules such as AppArmor, SELinux. Docker supports LSMs.

- people generally don't want to write custom AppArmor custom profiles, syntax is not great (note to self: look up how to write AppArmor profiles )

### Docker and seccomp

- seccomp is a Linux kernel security module

- Docker has a default whitelist, which prevents for example cloning a user namespace inside a Docker container
( note to self: look up why this is a weak spot for Linux kernel vulns )


### Docker security in the future

- one sad thing: containers need to run as root
- why: to write to sys/fs for cgroups, we need to be root

- cgroups cannot be created without ``CAP_SYS_ADMIN``

- cgroups namespaces: a potential solution to having to be root when creating a cgroup, but turns out this concept just limits what cgroups you can see

- there are patches in the kernel to make it possible to create cgroups with other users, but not yet


 




