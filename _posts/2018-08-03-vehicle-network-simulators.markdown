---
layout: post
title:  "Application Layer Simulators for Vehicle Networks"
date:   2018-08-03 15:04:00 +09
categories: VSimRTI VANET
---

With development of intelligent transport system applications, a series of simulation platforms began development to allow modeling new software applications in vehicle. There is now a clear split between simulation environments built with focus on communications and mobility modeling (OVNIS and VEINS), and new simulations tacks that have additional programming to allow running vehicle applications on top of existing mobility and communications models (iTetris and VSimRTI). 
Depending on the format of simulation, most environment bundle similar components to complete simulations. For platforms focused on communications, this the existing SUMO platform for mobility models, combined with NS-2, NS-3, SWANS or similar environment for communications facilities. Both OVNIS and VEINS use this pairing. Application layer simulators take these components and pair them with additional programming to host external application code, as well as managing data and clock syncing among all connected environments. Comparing performance, based on the above feature requirements, VSimRTI proved most compatible for the intended use. Below is a summary of features and the architecture each framework provides.

# OVNIS

Developed the University of Luxembourg in 2011, OVNIS is the least feature complete. The project integrate SUMO mobility data and the NS-3 network simulator. OVNIS does support active manipulation of simulation through web sockets, but has not received a code update since 2015, with documentation dating to its original publication in 2011. Mac and Linux platforms are supports for running simulations.

# Veins

Veins was first released in 2011. It is fully open source under the GPL license and has an active development community. Unlike OVNIS and iTetris; Veins combines SUMO mobility data, with the OmNet++ simulator. which is considered to have a less steep learning curve, compared to NS-3. Veins also has multiple prepackaged extensions to support simulating LTE radio networks as well as shadowing and path loss caused by city buildings – features lacking in both OVNIS and iTetris, without the addition of NS-3 LTE extensions and custom programming. Another important distinction of using Veins, is that it does not provide a fully developed application runtime environment. Instead, users can run any application, as long as the application is written in the C++ language and properly associates to an entity running from within the simulation (vehicle, roadside unit, or similar). Below is a diagram, provided by Veins, to outline the simulators modular functionality. 

![Veins](https://github.com/stevenplatt/stevenplatt.github.io/blob/master/assets/img/2018_08_03_veins.png)

# iTetris

Released in 2013, iTetris combines, SUMO mobility data with the NS-3 network simulator. iTetris does not include a GUI for visualizing trace and communication data, and support running simulations only on a Linux platform, but the system claims to be language agnostic for its vehicle application framework, and explicitly states support for simulating vehicle applications written in C++, java, and python; a large advantage over VSimRTI, which can only support application written in java. Features and tutorials are tailored for smart transport simulations, such as emissions and congestion models. Another differentiation vs the closed source VSimRTI, all code of the iTetris platform is open source and can be modified and used without special license. Figure 3 shows the structure of the iTetris simulator. 

![iTetris](https://github.com/stevenplatt/stevenplatt.github.io/blob/master/assets/img/2018_08_03_itetris.jpg)

# VSimRTI

VSimRTI is the newest simulation framework and is also most flexible. It supports SUMO traffic data, combined with NS-3, OmNet++, or SNS network simulators [27]. This is important, since it allows changing the communications layers to suite the experiment or custom code being deployed. The VSimRTI framework is also best at visualizing data with support for Open Street Maps traffic overlays. VSimRTI supports Windows, Mac, and Linux. Unlike the other simulators listed, VSimRTI is not open source, and running the simulator requires a physical license by granted by the Fraunhofer institute, who supports the simulator. VSimRTI relies on a few open source projects for the core of it simulation environment, but a large portion of the simulators code is proprietary and encompasses application code written to simulate vehicle and roadside unit operating systems. This includes items such as minimum following distance, how quickly vehicle speed is adjusted, and providing GPS, messaging, and other application functionality to entities in simulation. Figure 4 shows the VSimRTI runtime infrastructure and the open source components on which it is based. 

![vSimRTI](https://github.com/stevenplatt/stevenplatt.github.io/blob/master/assets/img/2018_08_03_VSimRTI_logo.png)
