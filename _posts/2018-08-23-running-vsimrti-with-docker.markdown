---
layout: post
title:  "Run VSimRTI in the Browser with Docker"
date:   2018-08-23 15:04:00 +09
categories: Docker VSimRTI
---

This post includes instructions for running the VSimRTI vehicle network simulator in the browser, using docker. This setup can be used to isolate a VSimRTI installation from your local machine, to prevent application conflicts. 

I won’t detail the installation of Docker, or management of docker images in this post however. It is important to note that when running VSimRTI for the first time, it is required to have a licensed issued to run simulations. This license is tied to your hardware configuration –  fortunately, executing the “firstrun.sh” script of VSimRTI, within Docker; it still sees the native hardware. Simulations can be run within Docker, or directly on the host computer, without a new license.

## Obtaining a License

When downloading the VSimRTI application, running the “firstStart.sh” from its root folder generates a text file with system info, including CPU core, operating system, and RAM installed. After emailing this file to the VSimRTI email distribution (vsimrti@fokus.fraunhofer.de), a username is issued with an assigned expiry date – the expiry can be extended with an additional email request.

**Note**: Running the firstStart.sh file requires root access (sudo) the java runtime. This is not installed by default on Ubuntu 16.04 LTS, I installed it using commands:

```
#add the openjdk repository and install open jdk
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-8-jdk
#add linux environment variable for java installation
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk 
```

## Running Ubuntu in the Browser

The first hurtle of making the simulation portable was to get an Ubuntu image running within Docker while having a full desktop UI. This was done using X11, VNC, Apache Server, and LXDE desktop on an Ubuntu 16 LTS core. The origins of this Ubuntu base can be viewed on GitHub.  From this core Ubuntu instance additional commands were added to the Docker file to install Java, Git, SUMO, Firefox, and to download, unzip, and place the VSimRTI files on the desktop. When running the Docker image, the included Apache installation and X11 make the Ubuntu LXDE desktop available at port 80 of the Docker machine. This creates a fixed package, that could be run, torn down, and shared between anyone who has simulations to run in VSimRTI.
Once connected to the running Ubuntu desktop, simulation files can be pulled down and run on the Docker machine, using Git. The below screenshot shows the VSimRTI “Tiergarten” example running within Firefox on the Docker Ubuntu image.

To pull the docker image:

```
docker pull telecomsteve/vsimrti-web
```

To run the image:

```
docker run -it –rm -p 6080:80 telecomsteve/vsimrti-web
```

Access the Ubuntu GUI from the browser:

```
http://127.0.0.1:6080/
```

![screenshot](https://raw.github.com/stevenplatt/docker-vsimrti-web/master/screenshots/vsimrti-web.jpg?v1)

This short post leaves out earlier information, such as how the simulations were configured. 
Assuming that there are existing simulation scenarios that need to be imported - these can be pulled from git, or downloaded to the running docker instance as done on a standalone PC. 

Below are the full contents of the docker file which is being downloaded in the instructions above. This source file is also available on Github, and can be forked and modified directly, as the GitHub version will have the latest version of the Docker buildfile. 


## The Docker Build File - [GitHub Source](https://github.com/stevenplatt/docker-vsimrti-web)

```
FROM ubuntu:16.04
LABEL maintainer="steven@telecomsteve.com"

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
 dialog wget unzip nano git \
 sumo sumo-tools sumo-doc

# jdk 8 install
RUN echo "oracle-java8-installer shared/accepted-oracle-license-v1-1 select true" | sudo debconf-set-selections
RUN apt-get install -y oracle-java8-installer

# vsimrti additional packages
RUN wget https://www.dcaiti.tu-berlin.de/research/simulation/download/get/vsimrti-bin-17.0.zip
RUN unzip vsimrti-bin-17.0.zip -d /root/Desktop
RUN rm vsimrti-bin-17.0.zip
RUN chmod +x /root/Desktop/vsimrti-allinone/vsimrti/firstStart.sh
RUN bash /root/Desktop/vsimrti-allinone/vsimrti/firstStart.sh

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

## Integrating the NS3 Simulator with VSimRTI - [GitHub Source](https://github.com/stevenplatt/docker-vsimrti-web)

To get the NS3 build included in the docker container a few additional software is installed, and the NS3 installer shell script is called to completed the NS3 build. These additional Docker build commands are added to the build file reference previously. 

```
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
 dialog wget unzip nano git libprotobuf-dev gcc g++ bison flex lbzip2 libxml2-dev rsync libsqlite3-dev patch

# vsimrti additional packages
RUN git clone https://github.com/stevenplatt/vsimrti-scenarios.git /root/Desktop/upf/

# NS3 install
RUN yes "y" | bash /root/Desktop/upf/vsimrti/bin/fed/ns3/ns3_installer.sh
```

## Testing NS3 Integration with Logging

After bundling NS3 into the Docker build, detailed communications layer logging can be enabled by modifying the vsimrti/etc/logback.xml file, to set NS3 logging to be enabled, with log level of “DEBUG”. Finally, the vsimrti_config.xml file within each scenario folder was updated to enable use of NS3 for communications in the simulation, and to disable the default enabled ‘SNS’ communications simulator. Doing this allowed using propagation models from NS3 and get full communication logging as shown below.

Logging Before: 

```
2018-04-12 16:29:13,270 INFO  SnsAmbassador - Send v2xMessage.id=19 from node=rsu_0 as Topocast (singlehop) @time=18.000,000,000 s

2018-04-12 16:29:13,271 INFO  SnsAmbassador - Receive v2xMessage.id=19 on node=veh_0 @time=18.000,900,000 s

2018-04-12 16:29:13,271 INFO  SnsAmbassador - Receive v2xMessage.id=19 on node=veh_1 @time=18.000,900,000 s

2018-04-12 16:29:13,271 INFO  SnsAmbassador - Receive v2xMessage.id=19 on node=tl_0 @time=18.000,900,000 s

2018-04-12 16:29:13,274 INFO  SnsAmbassador - Send v2xMessage.id=20 from=veh_2 as Geocast (geo routing) @time=19.000,000,000 s
```

Logging After: 

```
2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - ProcessMessage VehicleMovements at t=75000000000

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - Update Vehicle Positions

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - UpdateNode : ID[int=veh_2, ext=3]

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - Pos: x(918.2701732605929) y(173.16697777435184) Point2D.Double: GeoPoint{lat|lon=[52.513036,13.330212]}

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - UpdateNode : ID[int=veh_3, ext=4]

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - Pos: x(816.3387485357234) y(162.39611352980137) Point2D.Double: GeoPoint{lat|lon=[52.512918,13.328714]}

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - UpdateNode : ID[int=veh_0, ext=1]

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - Pos: x(903.0686549004749) y(174.82249094638973) Point2D.Double: GeoPoint{lat|lon=[52.513047,13.329987]}

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - UpdateNode : ID[int=veh_1, ext=2]

2018-04-12 16:35:15,786 DEBUG Ns3Ambassador - Pos: x(865.8905054948991) y(167.60215506237) Point2D.Double: GeoPoint{lat|lon=[52.512975,13.329442]}

2018-04-12 16:35:15,787 DEBUG Ns3Ambassador - ProcessTimeAdvanceGrant at t=75000000000

2018-04-12 16:35:15,787 DEBUG Ns3Ambassador - Requested next_event at 75000000000
```
