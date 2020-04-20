---
layout: post
title:  "Review of Vehicle Network Standards and Use Cases"
date:   2018-08-03 15:04:00 +09
categories: VANET V2X
---

The development of simulation platforms has followed closely the specifications outlined by standards bodies dating back to 2010. For vehicle network applications, these tentpole standards have been the ETSI Intelligent Transport Systems definition, and later expansions in the form of ITU-R Intelligent Transport Systems, LTE Release 14, and LTE release 15. 

# ETSI Intelligent Transport Systems
In 2010, ETSI defined its first set of Intelligent Transport Systems (ITS) standards [22]. The definition initially outlined six categories and provided examples of intelligent transport application:

**Active Road Safety**: Slow vehicle warning, intersection collision warning, emergency vehicle warning

**Cooperative Traffic Efficiency**: Enhanced route guidance, detour notification

**Cooperative Local Services**: Point of interest notification, media downloading, parking access control

**Global Internet Services**: Fleet management, Insurance services

**Hazardous Location Notification**: Road work warning, emergency brake light notification

**Signage Applications**: In vehicle speed limit display, signal violation (safety)

These categories of Intelligent Transport Systems are one of the earliest uses of the term. Today ETSI is actively updating its standards for ITS, which now include defining applications of vehicle cooperative adaptive cruise control (C-ACC), vehicle platooning, and vulnerable road users (VRU) [23] [24]. These updates are expected to be published as “Release 2”, with the original document being “Release 1”.

# ITU-R Intelligent Transport Systems

Prior to releases 14 and 15 of the LTE standard by the 3GPP, the United Nations ITU-R published guidance M.1890 (ITUR11-1890) in April 2011 [20]. The document titled “Intelligent Transport Systems - Guidelines and objectives” outlined eight classes of application for network connected transportation systems. These broad categories have since been extended, further defined and implemented in the market in the year after. These eight use classes are: 

**Advanced Vehicle Control Systems**: Collision avoidance, enhanced driver vision, pre-crash restraint deployment, and automated road systems. 

**Advanced Traffic Management Systems**: Congestion control, traffic control, emissions and parking management. 

**Advanced Traveler Information Systems**: Travel guidance, route guidance, and ride matching to facilitate ride sharing services.

**Advanced Public Transportation Systems**: Public transit automation and personalization.

**Advanced Fleet Management**: Vehicle mileage and fuel reporting, international border clearance and safety inspections. 

**Emergency Management Systems**: Public safety and emergency notification services. 

**Electronic Payment Services**: Electronic payment for toll, parking and other vehicle services.

**Pedestrian Supporting Systems**: Walking directions and vehicle-pedestrian collision avoidance.

# 3GPP LTE Release 14 

In 2015, under LTE release 14 technical standards; the 3GPP finalized Technical Specification (TS) 3GPP16-22885, which outlined performance and use requirements for V2X applications in several road conditions. This specification includes speed, distance, latency, and reliability measure focused on driver awareness and warning systems, with reliability requirements as strict as up to 95% [17]. This specification assumed level 1 vehicle communications, using CAM (cooperative awareness message) and DENM (decentralized environmental notification message) messages. The CAM and DENM message have since become a standard format available in a few V2X application simulators, such as VSimRTI. Full detail of TS 22.885 use case specifications are below: 

[Speed Table]

# 3GPP LTE Release 15

As an enhancement of 3GPP16-22886, the 3GPP later published 3GPP17-22186 as an enhancement for V2X services under LTE release 15. This update allowed more specific requirements among different vehicle network messages, not directly tying them to the environment in which they are deployed. This specification covered five areas [18]: 

**Generic Messaging**: General messaging, applicable in all V2X scenarios. Interworking, multi-RAT, and routing messages. 

**Vehicle Platooning**: Enabling vehicles to form a platoon and follow each other in a coordinated manner, with a shorter follow gap than usual.

**Advanced Driving**: Exchange of sensing data to increase the perception of connected vehicles.

**Extended Sensors**: Allowing semi-autonomous and fully-autonomous driving, by allowing vehicles to synchronize sensor and trajectory data.  

**Remote Driving**: Enables remote driving of vehicles operating in a dangerous environment, or operation of a vehicle for passengers who cannot operate it themselves. 

Within each of the above categories, 3GPP17-22186 outlines a similar range of network and use case performance requirements that include: payload size, end-to-end latency, reliability (%), data rate (Mbps), communications range (meters), and transmission rate.

# Example Research - Project 5GCar

Funded by the European Commission, the 5GCAR project is a public-private-partnership (PPP) developed to bring together the automotive and communications industries, with research institutions to develop next generation connected vehicle and intelligent transport applications enabled by 5G technologies. In its most recent deliverables, Project 5GCAR outlines five use cases for specific development [16]:
Cooperative Maneuver: Sharing of local data for driving intention and trajectory. This data is used to negotiate interaction among groups of vehicles. 
Cooperative Perception: Sharing of data derived from various sensors. These sensors can be installed to the vehicle, road, or other positions. Development in this area focuses heavily on delivering better than line-of-sight vision to smart vehicles. 
Cooperative Safety: Sharing of data, optimized for detection of road hazards and safety of other road users, such as pedestrians and cyclists. 
Autonomous Navigation: Centralized and or distributed processing of maps and routes. These routes are derived from shared vehicle and road network data, and combined with traditional remote sensing and navigation systems.
Remote Drive: Controlling a vehicle remotely to enable driving without a physical operator present. This includes actuating all vehicle function remotely, inclusive of braking, steering, and acceleration.

## Toy Car Lane Merge Model from CTTC

Today, research is being conducted at Centre Tecnològic de Telecomunicacions de Catalunya (CTTC) under the 5GCar project to prove an implementation of the cooperative maneuver use case in connected cars. The hardware implementation under development at CTTC uses a sensing camera to detect vehicle position, while feeding the data to a network controller for processing. Additionally, the network controller, with visibility of the total network state, applies algorithms and issues command and control to vehicles to aid successful navigation and collision avoidance in the cooperative maneuver use case.

Modeling networks on these use cases requires modeling strict vehicle-to-vehicle and vehicle-to-infrastructure communication layers, but also, additional capability to model vehicle mobility, external sensor and camera data, as well as allowing remote interfaces for network controllers and application code to execute real-world command and control during simulations.

[Toy Car Image]

## 5GCAR: Cooperative Maneuver Use Case

The toy car model built at CTTC is an example of the Cooperative Maneuver use case, presented in Project 5GCar. Within European Commission requirements outlined for project 5GCAR; vehicles operating in this use case satisfy interaction conditions summarized below [16]: 

**Precondition**: Vehicles are in physical proximity to each other, have autonomous driving capabilities, satellite navigation, and are equipped with wireless communications. Participating vehicles are authenticated. 

**Triggering Event**: A vehicle wants to join traffic, and merge into a lane.

**Actors**: Traffic vehicle (required), merging vehicle (required), infrastructure sensors (optional), application server (optional). Traffic vehicles must be network enabled, merging vehicles may be network enabled, or not.

Within these conditions, Project 5GCAR intends that wireless communications systems be used to supplement Advanced Driver Assistance Systems (ADAS), which is most commonly implemented using radar, lidar, and vision cameras; to allow better than Line of Sight (LOS) vision and enhanced localization. 
