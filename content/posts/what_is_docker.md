+++
date = '2025-07-08T21:35:10+02:00'
draft = true
title = 'What is docker and why should you care?'
+++

## Introduction

At my work it continuously manifests itself that our future lies with docker containers.
As a matter of fact even more confusing terms are being used. They talk about 'CRC' aka
'Code Ready Containers', Kubernetes clusters and other things that make my head spin so
violently that I have to hold onto my chair, else I would topple over.

In case it is not clear already, I am not in the slightest familiar with these technologies.
But this is no reason to fret! Tt is my believe that the IT sector means constant
learning and so I welcome this step towards 'modern' tools. (In an industry that is evolving
 as quickly as it does one should always be careful when using the word modern.)
Especially, since my company has a tendency to lack behind - significantly - when it comes to technology.

So I started to look into Docker containers and first tried to answer the question:

##  "What are they and whey should I care".

To understand why we should "go there" we should start by understanding where we are
coming from...

### The simple beginnings

In the past things were much simpler. Every server had exactly one operating system
installed. Let it be a version of Windows (Server) or a flavor of Linux. Everything
would be controlled by one OS.

This simplicity would come with a lack of flexibility. What if you have Linux installed
but need Windows for an application or vice versa? What if you need more computational power or
less? Would you want to invest in new hardware and have to maintain it in that case?

The issues do not stop there. On application level there are further issues when you have
just one big server. What if you have conflicting dependencies between the software you use?
Maybe your Wordpress and your data base cannot come to terms about drivers they need.

### Divide and conquer

And with that the stage opens for virtual machines. They use something called a hypervisor
to divide a computers resource for multiple guest operating systems. They come in two
different flavors. Either they function as a lightweight operating system and run directly
on the hardware or they run on a host operating system.

All of a sudden, you are able to quickly react to changing needs. You can create a new
virtual machine on your existing hardware that runs Windows Server or Linux. If one
machine needs more power, you can simply assign more resources (if available), no need
to get new hardware.

In a sense, Virtual Machines are virtualizing hardware.

### One step back and one leap forward

Docker now goes one step further ... or maybe back. You no longer need a hypervisor.
Instead a single operating system on your hardware suffices. Because what docker is doing
is to virtualize the operating system instead of the hardware.

The docker engine runs on the host operating system and hosts itself so called docker
"containers". Which could be described as isolated micro computers. They can have their
own CPU, OS, Memory and Network.

## Why care?

Their strongest selling point is that they are lightweight. Once you have the Docker engine installed
you for example pull the docker image for Ubuntu - which is only 77mb in size - and you have
it running within seconds. Installing Ubuntu on a virtual machine takes magnitudes longer.

But how is that even possible? The secret is that Linux based docker containers share their
Kernel with the host operating system. This brings one big drawback with it and
that is that a Linux OS can only host Linux docker container whereas a Windows host can only
run Windows docker container. (Not taking into account that you could use Windows Subsystems
for Linux).

The biggest advantage is yet to come. Technically I have already mentioned it, but I want to
emphasize it .. strongly! They are isolated and therefore highly portable.

A scene like the following happens every day around the globe and is with a high probability
also familiar to you:

  Developer: Finally I am done with this feature. All tests are green. Time to give it up for review.

  * other Developer / Tester tries to run the code and it fails *

  Developer: Don't know the problem. IT RUNS ON MY MACHINE.

Who knows how much money / time is lost because of differing runtime environments. May it be the
environment variable that you have changed months ago or this one dependency that you forgot
to document. Maybe you run a different version of *insert about anything* or who knows what might be the reason.

This will be a thing of the past. Because everything you need for a script to run must be part
of a docker image. When it runs on your machine, it will run on every other machine. Because
all dependencies and all those little tweaks must be part of the docker image.

Quick break to explain terminology: an image is the abstract definition of the environment and
every instance of this image is called a docker container. Cool? Cool!

You wanna know what else is neat? It's this thing about isolation. You can have different parts of
your server run in their own docker container and have those containers communicate with one another.
Each having their needed dependencies in the version they need.

One component needs an update either for itself or a dependency? No problem. Your other
components will not be affected. They might not even notice anything being different.

## Conclusion

It's time to wrap this up. This article was only intended to be a brief introduction into
the topic of docker and as such can only be seen as a starting point.

Most information stem from this NetworkChuck video: https://www.youtube.com/watch?v=eGz9DS-aIeY
Check it out if you want to see some Docker container in action.

In case you jumped right to the end for the answer to the question why you should care. Here are
the 3 main reasons I see:
Docker containers are
* lightweight
* isolated
* highly portable
