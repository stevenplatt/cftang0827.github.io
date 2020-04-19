---
layout: post
title:  "Running VSimRTI with Docker"
date:   2018-08-23 15:04:00 +09
categories: Docker VSimRTI
---

This post includes instructions for running the VSimRTI vehicle network simulator in the browser, using docker. I won’t detail the installation of Docker, or management of docker images in this post however. It is important to note that when running VSimRTI for the first time, it is required to have a licensed issued to run simulations. This license is tied to your hardware configuration –  fortunately, executing the “firstrun.sh” script of VSimRTI, within Docker; it still sees the native hardware. Simulations can be run within Docker, or directly on the host computer, without a new license.

To pull the docker image:

```Bash
docker pull telecomsteve/vsimrti-web
```

## Running Ubuntu in the Browser

The first hurtle of making the simulation portable was to get an Ubuntu image running within Docker while having a full desktop UI. This was done using X11, VNC, Apache Server, and LXDE desktop on an Ubuntu 16 LTS core. The origins of this Ubuntu base can be viewed on GitHub.  From this core Ubuntu instance additional commands were added to the Docker file to install Java, Git, SUMO, Firefox, and to download, unzip, and place the VSimRTI files on the desktop. When running the Docker image, the included Apache installation and X11 make the Ubuntu LXDE desktop available at port 80 of the Docker machine. This creates a fixed package, that could be run, torn down, and shared between anyone who has simulations to run in VSimRTI.
Once connected to the running Ubuntu desktop, simulation files can be pulled down and run on the Docker machine, using Git. The below screenshot shows the VSimRTI “Tiergarten” example running within Firefox on the Docker Ubuntu image.

To run the image:

```Bash
docker run -it –rm -p 6080:80 telecomsteve/vsimrti-web
```

Access the Ubuntu GUI from the browser:

```HTML
http://127.0.0.1:6080/
```
This short post leaves out earlier information, such as how the simulations were configured. 
Assuming that there are existing simulation scenarios that need to be imported - these can be pulled from git, or downloaded to the running docker instance as done on a standalone PC. 

Below are the full contents of the docker file which is being downloaded in the instructions above. This source file is also available on [Github](https://github.com/stevenplatt/docker-vsimrti-web), and can be forked and modified directly, as the GitHub version will have the latest version of the Docker buildfile. 


## The Docker Build File

```Bash
FROM ubuntu:16.04
LABEL maintainer="TelecomSteve"

RUN sed -i 's#http://archive.ubuntu.com/#http://tw.archive.ubuntu.com/#' /etc/apt/sources.list

# base install packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends software-properties-common \
    && add-apt-repository ppa:fcwu-tw/ppa \
    && add-apt-repository -y ppa:sumo/stable \
    && apt-add-repository -y ppa:webupd8team/java \
    && apt-get update \
    && apt-get install -y --no-install-recommends --allow-unauthenticated \
        supervisor \
        sudo vim-tiny net-tools lxde x11vnc xvfb python-software-properties debconf-utils \
        firefox nginx python-pip python-dev build-essential \
        mesa-utils libgl1-mesa-dri dbus-x11 x11-utils \
        dialog wget unzip nano git libprotobuf-dev rsync libsqlite3-dev patch lbzip2

#SUMO installation
RUN apt-get install -y sumo sumo-tools sumo-doc

# jdk 8 install
RUN echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 select true" | sudo debconf-set-selections
RUN apt-get install -y oracle-java8-installer

# Omnet++ additional packages
# RUN apt-get install -y gcc g++ bison flex perl tcl-dev tk-dev blt libxml2-dev zlib1g-dev \
# && apt-get install -y doxygen graphviz openmpi-bin libopenmpi-dev libpcap-dev \
# && apt-get install -y autoconf automake libtool libproj-dev libfox-1.6-dev \
# && apt-get install -y libgdal-dev libxerces-c-dev qt4-dev-tools libgdal1-dev libwebkitgtk-1.0-0

# vsimrti additional packages
RUN git clone https://github.com/stevenplatt/vsimrti-scenarios.git /root/Desktop/upf/

# NS3 install
# RUN yes "y" | bash /root/Desktop/upf/vsimrti/bin/fed/ns3/ns3_installer.sh

# tini for subreap
ARG TINI_VERSION=v0.9.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

ADD image/usr/lib/web/requirements.txt /tmp/
RUN pip install setuptools wheel && pip install -r /tmp/requirements.txt
ADD image /

# desktop customization
ADD /desktop/panel /tmp/
ADD /desktop/slate.png /tmp/
RUN rm /etc/xdg/lxpanel/default/panels/panel
RUN rm /etc/xdg/lxpanel/LXDE/panels/panel
RUN cp /tmp/panel /etc/xdg/lxpanel/default/panels/panel
RUN cp /tmp/panel /etc/xdg/lxpanel/LXDE/panels/panel

EXPOSE 80
WORKDIR /root
ENV HOME=/home/ubuntu \
    SHELL=/bin/bash
ENTRYPOINT ["/startup.sh"]
```
