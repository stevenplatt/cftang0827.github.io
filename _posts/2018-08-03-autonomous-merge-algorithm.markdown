---
layout: post
title:  "Autonomous Vehicle Merge Algorithm for 802.11p VANETS"
date:   2018-08-03 15:04:00 +09
categories: VSimRTI VANET Algorithms
---

With the simulation environment chosen, this section covers the details of the algorithm planned for managing lane merges of approaching vehicles in a custom-built simulation scenario, named “Castelldefels”.
The Castelldefels simulation assumes 802.11p communications, rather than a cellular network. This allows the same simulation to be built with physical test hardware or expanded by the CTTC if required.

# Algorithm Message Flow

Based on vehicle routes calculated through SUMO, the expectation is that a roadside unit at the junction point will sit listening always for oncoming vehicles. Vehicles in the simulation are limited to two predefined routes – I refer to them as routes “A”, and “B”. All vehicles in the simulation are broadcasting beacons using 802.11p as they drive the route. Once in close range, the roadside unit will begin receiving these broadcasts.
The algorithm gives priority to vehicles entering on route B. Vehicles on route A pass and enter the roundabout unrestricted, until messages arrive to the RSU from vehicles in range on route B. After the first message arrives – the RSU sends a control command to the vehicle on route A to ‘stop’, until it no longer receives the broadcast beacons from the vehicle on route B. At that time, the RSU send a second control command to the route A vehicle to ‘drive’.

# Algorithm Logic

The above image shows the full detail of the algorithm. As part of the vehicle operating system implemented by VSimRTI, the vehicles are deployed with controls that manage the acceleration, top speed, and minimum follow distance of vehicles. They are essentially driving autonomously in a platoon, along the simulation routes, until the algorithm controls are applied. For this reason, no controls or settings relating to these items are included in the algorithm. I can limit it only to the management of the merge action. 
Note that the algorithm code runs within the ‘Castelldefels RSU’ java class on the roadside unit. For additional context I represent again the logic in written form below:

