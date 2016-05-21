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

