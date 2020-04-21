---
layout: post
title:  "Autonomous Vehicle Merge Algorithm for 802.11p VANETS"
date:   2018-08-03 15:04:00 +09
categories: VSimRTI VANET Algorithms
---

With the simulation environment chosen (VSimRTI), this post covers the details of the algorithm planned for managing lane merges of approaching vehicles in a custom-built simulation scenario, named “Castelldefels”.

The Castelldefels simulation assumes 802.11p communications, rather than a cellular network. This allows the same simulation to be built with physical test hardware or expanded by the CTTC if required.

# Algorithm Message Flow

Based on vehicle routes calculated through SUMO, the expectation is that a roadside unit at the junction point will sit listening always for oncoming vehicles. Vehicles in the simulation are limited to two predefined routes – I refer to them as routes “A”, and “B”. All vehicles in the simulation are broadcasting beacons using 802.11p as they drive the route. Once in close range, the roadside unit will begin receiving these broadcasts.

![message_flow](/assets/img/message_flow.jpg)

The algorithm gives priority to vehicles entering on route B. Vehicles on route A pass and enter the roundabout unrestricted, until messages arrive to the RSU from vehicles in range on route B. After the first message arrives – the RSU sends a control command to the vehicle on route A to ‘stop’, until it no longer receives the broadcast beacons from the vehicle on route B. At that time, the RSU send a second control command to the route A vehicle to ‘drive’.

# Algorithm Logic

The above image shows the full detail of the algorithm. As part of the vehicle operating system implemented by VSimRTI, the vehicles are deployed with controls that manage the acceleration, top speed, and minimum follow distance of vehicles. They are essentially driving autonomously in a platoon, along the simulation routes, until the algorithm controls are applied. For this reason, no controls or settings relating to these items are included in the algorithm. I can limit it only to the management of the merge action. 

![castelldefels_algorithm](/assets/img/castelldefels_algorithm.png)

Note that the algorithm code runs within the ‘Castelldefels RSU’ java class on the roadside unit. For additional context I represent again the logic in written form below:

![algorithm_logic_alt](/assets/img/algorithm_logic_alt.png)

# Environment Map

Applying the autonomous merge algorithm in virtual space, figure 7 shows the layout of the Castelldefels simulation. Note that vehicles in the diagram are not to scale. The diagram shows physical representation of vehicles concurrently arriving on from opposing routes A and B. The controlling roadside unit is also displayed. The blue circle surrounding the center of the diagram represents the communications range in which the roadside unit receive ad hoc beacons from arriving vehicles. This assumes a standard transmit and received powers along with uniform path loss. 
Beyond these three actors in the environment, I show representation of the beacon data flow, from the vehicles to the tower, and corresponding stop and drive response sent in return. Last, there is the target merge zone, the roundabout, marked with a blue triangle.

[Environment Map]

# Java Application Class Map

Table 2 shows which java application classes are running on each actor in the simulation. All vehicle and all roadside units are assumed identical in the simulation. 

The roadside unit has the simpler structure of the entities in the environment. Its programming is being housed in a single Java class, named “Castelldefels RSU”. This coding equips the roadside unit with 802.11p ad-hoc network abilities, along with the algorithm logic for receiving vehicle beacon data, parsing its contents, and responding with the “Stop” or “Drive” commands shown in the previous figure 7.

Programming on the vehicle includes two java classes, these are the ‘Slow Down App’, and ‘Network Merge Assist’ classes. The Network Merge Assist class works similar to the Castelldefels RSU application class, it equips the vehicle with 802.11p networking, and manages the queuing and broadcast of vehicle data, such as location and route, while listening for responses from a roadside unit, and when the message is received, it parses the Stop or Drive message. If a Stop message is received, this class calls the Slow Down App, which brings the vehicle to a stop using internal vehicle control messages.

[Table]

# Autonomous Merge Fallback

VSimRTI has thorough documentation of its code capabilities, Java API, and provides explicit code samples when downloading the simulator. In reading the provided VSimRTI documentation, the Java API follows very closely the DENM and CAM messaging standards outlined in LTE release 14 and may have limits to the amount and types of data these messages can carry between vehicles in simulation. Because source code for VSimRTI message handling is not open source - I have added additional fallback programming, based on known functions demonstrated in the Barnim simulation bundled with VSimRTI. In this scenario, road sensors are used in place of direct message exchange between vehicles and RSU. For the Castelldefels simulation implementation, this means a sensor area covering the area of the roundabout signals a speed event to all oncoming vehicles, to force a speed reduction as they navigate the known hazardous merge. Coverage for this sensor notification area is set equal to that shown in figure 7. Code for the fallback sensor event is covered in later section “Signaling a Sensor Event”, with additional coding contained in the java class “Network Merge Assist” running on simulated vehicles. 

With the algorithm logic, environment, application classes and fallback functions mapped; I move onto building the simulation within VSimRTI. 

