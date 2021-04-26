---
title: Docker overview
layout: post
date: '2021-04-24T20:35:44.899Z'
theme: gaia
header: 'Docker'
style: 'header { font-weight: bold; font-size: 2em; text-align: center; }'
---

## OS-level virtualization

> Note: This document is marp compatible.

---

## Intro

Similar technologies existed for decades:

* zones (Solaris 10, February 2004)
* jails (FreeBSD 4.0, March 2000)
* chroot (Unix v7, 1979)
* user access separation (multics, unix, early 1970s)

Most Linux required features existed since 2.6  
Docker reuses existing standards/technologies/philosophy.

* _That's it? What's the catch?_

---

## Ecosystem
- comprehensive documentation  
  - https://docs.docker.com/
- distinct *interface*:
  - CLI: docker, podman
  - clouds
- registries (hubs):
  - public: docker.io, quay.io, registry.fedoraproject.org
  - self-hosted
  - _localhost cache_

---

## Terminology
- image (`ls ~/.local/share/containers/storage/overlay/`)
- container
- hub (https://hub.docker.com/search?type=image)
- volume

> https://docs.docker.com/glossary/

---

## Under the hood

- [runC universal runtime](https://www.docker.com/blog/runc/)
  - split: kernel namespaces (net/pid/mount/user/networks...) (2002)
  - limit: cgroups (2007)
  - chroot (pivot root)
  - ...
- [OverlayFS (layers)](https://docs.docker.com/storage/storagedriver/overlayfs-driver/)

> https://www.codementor.io/blog/docker-technology-5x1kilcbow

---

## Key takeaway
![bg auto drop-shadow right:55%](https://upload.wikimedia.org/wikipedia/commons/0/09/Docker-linux-interfaces.svg)

- NO kernel inside
- SINGLE process


_From wikimedia -->_

---

## Hands on
```bash
docker search <name>
docker pull <NAME[:TAG|@DIGEST]>
docker run -it --rm -p <host_port>:<docker_port> -v <image> [cmd]

docker run -d <image>
docker attach

docker stop
docker start
docker log
```

Example: `docker run -p 8080:8888 -it --rm bash nc -l -p 8888`

---

## Dockerfile
> Refer: https://docs.docker.com/develop/develop-images/dockerfile_best-practices/
> Doc: https://docs.docker.com/engine/reference/builder/

| Notable keywords | | |
---|---|---
FROM | LABEL | **USER**
RUN | COPY/ADD | EXPOSE
ENV | (ENTRYPOINT) | CMD

Example: [ubuntu dockerfile](https://github.com/dockerfile/ubuntu/blob/master/Dockerfile)

---

## Quick build and share image

```
docker image build --rm -t test_image <path-to-dockerfile>
docker image save test_image -o test.tar
...
docker image load -i test.tar
```

> Consider using multi-stage builds.

---

## Exploring and housekeeping
```
docker info | less
```
```
docker history --no-trunc <imgname>
docker image inspect <imgname>
```
```
docker image ls
docker image rm <image_name>
docker image prune
```
```
docker container ps -a
docker container rm <container_id>
docker container prune
```
