---
layout: post
title:  "Docker Usage"
categories: coding
tags: docker
---

* TOC
{:toc}

### Commands

1. List all running container
```bash
sudo docker ps
```

2. List all container
```bash
sudo docker ps -a
```

3. Remove a container
```bash
sudo docker rm 6766afe8f0ad # here 6766afe8f0ad is the container ID
```

4. List all images
```bash
sudo docker images
```

5. Go into an existing container
```bash
# start the container
sudo docker start 6766afe8f0ad
# exec command in container
sudo docker exec -ti 6766afe8f0ad /bin/bash
# stop the container
sudo docker stop 6766afe8f0ad
```

6. Find nvidia drive version
```bash
nvidia_version=$(cat /proc/driver/nvidia/version | head -n 1 | awk '{ print $8 }')
echo $nvidia_version
```

#### Build image and Run
There should be a `Dockerfile` in the directory, in which there is command you need to install into the image. For example this `Dockerfile`:
```bash
FROM ubuntu
MAINTAINER github/gklingler

RUN apt-get update
RUN apt-get install -y mesa-utils
RUN apt-get install -y module-init-tools
RUN apt-get install -y glmark2
RUN apt-get install -y lsb-core libfontconfig1 libxrender1 libxtst6 libglu1-mesa libglib2.0-0 libsm6 xdg-utils wget
RUN cd /home/
RUN wget https://dl.google.com/dl/earth/client/current/google-earth-stable_current_amd64.deb
RUN dpkg -i google-earth-stable_current_amd64.deb

# install nvidia driver
RUN apt-get install -y binutils
ADD NVIDIA-DRIVER.run /tmp/NVIDIA-DRIVER.run
RUN sh /tmp/NVIDIA-DRIVER.run -a -N --ui=none --no-kernel-module
RUN rm /tmp/NVIDIA-DRIVER.run
```

Build the image with command
```bash
nvidia_version=$(cat /proc/driver/nvidia/version | head -n 1 | awk '{ print $8 }')
echo $nvidia_version
# We must use the same driver in the image as on the host
if test ! -f nvidia-driver.run; then
  echo here
  nvidia_driver_uri=http://us.download.nvidia.com/XFree86/Linux-x86_64/${nvidia_version}/NVIDIA-Linux-x86_64-${nvidia_version}.run
  wget -O nvidia-driver.run $nvidia_driver_uri
fi

sudo docker build -t opengl-nvidia:${nvidia_version} .
sudo docker tag opengl-nvidia:${nvidia_version} opengl-nvidia:latest
```

Then if you want to run the image, using
```bash
sudo docker run --privileged -e "DISPLAY=unix:0.0" -v="/tmp/.X11-unix:/tmp/.X11-unix:rw"  -i -t opengl-nvidia:latest /bin/bash
```



