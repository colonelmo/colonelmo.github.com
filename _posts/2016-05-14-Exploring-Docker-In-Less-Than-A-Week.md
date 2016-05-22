---
layout: post
title: Exploring Docker in less than a week
---

-----

<!--- TODO: docker compose, machine, swarm<<] --->

So yea I have finally started to write.
I'm gonna explore docker this week. You probably know about the hype/hate/criticism/etc. about docker so I'll do us all a favor and won't go through all that stuff yet another time.

Docker works with a client/server model but with a single executable which plays as both of those.
To run the docker server/daemon: ```docker -d```. Docker can listen on Unix sockets and TCP ports.

Docker containers are run as virtual hosts behind a bridged network which is implemented by the docker server. The containers are on the same subnet and can talk to each other directly but to go further, they have to pass through the network bridge, and this is done using a proxy when going the other way around.

Stateless (web) applications are good candidates to be run on docker, but those usually have states (configuration files, etc.) too. A good practice is to pass configurations as environment variables so that the same app can be run in deployment/development and the settings/assumptions about configurations can be changed easily. The environment variables that are passed to a container are also saved as metadata so that you'll re-create the same environment when restarting containers.

Docker runs containers which are created from container images which in turn are created from ```Dockerfile```s. Since each command in a dockerfile creates a new docker image, it's a good idea to keep the commands whose outcome differs from time to time near the bottom of the dockerfiles.

For particular actions (e.g. pushing to a repo), you need to authenticate yourself using docker cli. ```docker login $REPO_ADDRESS``` does this for you and creates auth tokens based on your info and uses them to authenticate you when needed. ```docker logout``` will simply delete the file.

Cpu usage limitations can be imposed on the containers using process cpu shares as a soft limit. The set of processor cores on which the process runs and the amount of memory and/or swap space that the process uses can also be specified as hard limits. To apply other limitations ulimit can be used, either in the form of --default-ulimit when launching the daemon, or as --ulimit when creating a container.

```docker stop``` sends a ```SIGTERM``` to the process. -t option to docker stop specifies a time for docker to wait for the process to gracefully exit. Also docker kill can be used to immediately kill a container.

Docker containers can be paused/unpaused without signal notifications. This is built on top of ```cgroup freeze```. After unpausing, the container's uptime is resumed.

```docker exec``` is handy tool for the situations when you need to actually enter the container and say, execute a number of shell command for debugging.```docker exec``` clearly depends on docker, but another linux native tool ```nsenter``` is still there for you if docker is not. Neither of this work as expected if there is no unix shell available inside the container. 

```docker stats``` provides a ```top```-like result about the running continaers. Docker api provides much richer information though. ```docker events``` does what you think it does and can come in handy when logging events related to creating, altering, etc. of the containers.

There are somewhat interesting relationships between the processes in the docker hierarchy. You'll have a good grasp of the situation by checking out ```ps axlfwww```, ```ps -ejH```, and ```pstree `pidofdocker` ```.

Docker uses ```libcontainer``` as the containerization engine since ```v0.9``` rather than ```LXC```. What docker uses as the containerization backend, ```libcontainer``` or ```LXC``` can be specified at run-time, but the two are not compatible and containers created using the ```LXC``` backend can not be run using the ```libcontainer``` backend and vice versa.

As you probably know, docker isolates the processes using linux cgroups. Every docker container is assigned a cgroup unique to that container. cgroups allow not only isolation, but better resource monitoring. With the execution driver set to LXC, many options such as ```iops``` can be passed to the container using ```--lxc-conf``` flag when creating the container. It's important to note that you're passing cgroup info directly to LXC driver when using ```--lxc-conf``` and docker won't know about any of it.

By default anything is priviledged in a docker container. You will execute processes with UID 0, which mean if any outbreak (from the container to the actual docker host) happens, you are _root_. Maybe it's a good idea to run your containers with a non-priviledged UID? I don't know. However the kernel will somehow do it's best to constrain the container e.g. by not letting it remove a kernel module, but even that won't be there with ```--privileged=true```. That's where ```--cap-add``` and ```--cap-drop``` enter to allow you to set particular kernel capabilities for the container that you are about to create. To deal with these security concerns in a better way, SELinux and AppArmor can be used. They can provide Mandatory Access Control to the system, which can in turn constraint the containers from say, alterint dangerous parts of the file system or getting ahold of a part of the file system by mounting it.

Let's have a dive in the source code now:


