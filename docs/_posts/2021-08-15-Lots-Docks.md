---
layout: post
title: "Lots O' Docks"
date: 2021-08-15 23:00:00 -0400
categories: sdc
---
### Docker Won't Give Up the Ghost
There was a lot of running hither and yon through documentation, error messages, Stack Overflow, and trying to find out where files go and don't go. I found out early in the week that the initial plan that I had, to deploy Node and Postgres in separate containers on separate EC2 instances was error riddled. The Docker Compose docs are quite adamant that the Docker Compose process is meant for single host deployments rather than multi-host deployments. [Docker Compose Docs](https://docs.docker.com/compose/)
Multi-host deployments in Docker are instead intended to utilize Docker Machine / Docker Swarm. At first it seemed that the build process for Docker Swarm was completely disconnected from Dockerfiles and docker-compose YMLs. And it can be, utilizing the 'docker service create' commands. But there is also a slick way to 'stack' and utilize docker-compose too, a process that I found necessary for Node. A fantastic resource here is ["Deploy a stack to a swarm"'](https://docs.docker.com/engine/swarm/stack-deploy/).
The Node deploy process was error ridden. It kept looking for Nodemon even though I had completely removed it from my code and repositories. Turns out that Docker was using cached files. The System Prune is a gamechanger!
[Docker System Prune](https://docs.docker.com/engine/reference/commandline/system_prune/)
[Docker Builder Prune](https://docs.docker.com/engine/reference/commandline/builder_prune/)

### Files Swirling Around
Though I invested a hearty amount of time in understanding Gluster, I'm still not sure that I like it or trust it. By the general conversation online, it seems that either Gluster or some kind of NAS are the usual methods of keeping volume data under control with Docker Swarms. Had this project been given fuller license to utilize AWS offerings, I think I would have gladly spun up something that way. There are some that say that Gluster is "better" than a NAS because it is less likely to have a single point of failure. But I think that is only true if the replication is dependably working. I would also very much like to see what Kubernetes has to offer this equation in the future.

### Won't Slow Down
I stress tested the four API routes on K6, utilizing the same intervals of requests per second as previously performed locally; so that I would be able to get a good ballpark of the difference between local and deployed. There are two major observations that I made. First, it seems as though the remote were taking real considerable amount of time to complete when done in the 1s or 10s. But then once I opened up to 100 and 250, the averages dropped considerably. By the time I got to 1000 on the remote tests, the averages times increased just slightly, but my constraint became my local machine rather than the server.

Comparisons of Local vs. Remote at 100 RPS:
(55ms is subtracted from the Remote numbers to account for the average ping between here and AWS east-1)

Product: 7.16ms Local, 14.90ms Remote
Products: 3.85ms Local, 16.97ms Remote
Related: 3.71ms Local, 9.16ms Remote
Styles: 7.52ms Local, 15.27ms Remote

Stress tests at 1000 (or close to it) RPS:

[Product Stress Test](https://drive.google.com/file/d/1eezLG42xif0w5GUvk9_RkHYg9b41bKtu/view?usp=sharing)

[Products Stress Test](https://drive.google.com/file/d/11H9h7-JXlC505LLVVbb7LxktNkCakGLS/view?usp=sharing)

[Related Stress Test](https://drive.google.com/file/d/1bSaJnef9cxC3oh4dTC2tgr3VXB-Xej6o/view?usp=sharing)

[Styles Stress Test](https://drive.google.com/file/d/1EO5kr09uWGM4FBPXHRSP9nQbnohy804-/view?usp=sharing)
