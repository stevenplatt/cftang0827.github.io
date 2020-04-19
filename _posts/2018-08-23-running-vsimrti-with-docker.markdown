---
layout: post
title:  "Running VSimRTI with Docker"
date:   2018-08-23 15:04:00 +09
categories: Docker VSimRTI
---

This post includes further research that was completed as part of experimentation - but were ultimately removed, as they contribute directly to experiment results. I won’t detail the installation of Docker, or management of docker images. It is important to note again that when running VSimRTI for the first time, it is required to have a licensed issued to run simulations. This license is tied to your hardware configuration –  fortunately, executing the “firstrun.sh” script of VSimRTI, within Docker; it still sees the native hardware. Simulations can be run within Docker, or directly on the host computer, without a new license.

To pull the docker image:

```Java
docker pull telecomsteve/vsimrti-web
```

## Running Ubuntu in the Browser

The first hurtle of making the simulation portable was to get an Ubuntu image running within Docker while having a full desktop UI. This was done using X11, VNC, Apache Server, and LXDE desktop on an Ubuntu 16 LTS core. The origins of this Ubuntu base can be viewed on GitHub.  From this core Ubuntu instance additional commands were added to the Docker file to install Java, Git, SUMO, Firefox, and to download, unzip, and place the VSimRTI files on the desktop. When running the Docker image, the included Apache installation and X11 make the Ubuntu LXDE desktop available at port 80 of the Docker machine. This creates a fixed package, that could be run, torn down, and shared between anyone who has simulations to run in VSimRTI.
Once connected to the running Ubuntu desktop, simulation files can be pulled down and run on the Docker machine, using Git. The below screenshot shows the VSimRTI “Tiergarten” example running within Firefox on the Docker Ubuntu image.


