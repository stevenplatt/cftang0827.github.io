---
layout: post
title:  "A Vehicle Telemetry Blackbox using Arduino and OBDII"
date:   2018-09-30 15:04:00 +09
categories: VANET Arduino
---

Before applying simulated configurations to real vehicles, a period of data collection is required. The following section proposes an arduino-based OBD II ‘black-box’ prototype to measures onboard sensors, such as speed, throttle pedal pressure, and odometer readings. Depending on the vehicle (and OBD II specifications thereof), additional measures such as braking pedal pressure may also be available. The prototype is capable of collecting these vehicles measures, and additionally collects vehicle location data, vehicle follow distance, and exchanges V2X messages through supplemental GPS, ultrasonic sensor, and 802.11-based “Xbee” modules. 

This prototype design allows real-world readings of the parameters implemented both in the java application simulated for the thesis, and its corresponding ‘mapping3’ file, used by VSimRTI to manage the simulation. Note that the hardware proposed is an extension of an existing design [source]. It is intended as a starting point for planning the hardware implementation of simulations done in software with VSimRTI. 

## OBD II Diagnostic Port

Introduced in United States in 1994, the on-board diagnostic port lets vehicle owners and repair centers, access diagnostic and status information from a cars´ various onboard computers. Implementation of the OBDII port varies between country region and vehicle manufacturer. Confirming what data is exposed for a specific vehicle can be found by researching the PID numbers (parameter identification numbers) supported by the vehicle, and the corresponding pins the data can be read from. In a number of late model vehicles, pins from the OBD II port can also be used to set ECU (electronic control unit) parameters, and even actuate vehicle electronics, such as power steering, and electronic throttle. In this prototype, custom code and wiring of the OBD II port is not required, instead, a data logging plug is connected (ELM327) to transmit readings to a the paired bluetooth master, controlled by an Arduino.

## Prototype Black Box Hardware

The full listing of measures and their corresponding VSimRTI parameter are shown in table below: 

### Telemetry Measures

Black Box Measures | VSimRTI Parameter Equivalents
------------ | -------------
Speed | acceleration, deceleration, maximum speed
Location | Route, Road, Position
Distance | Minimum Gap

### Black Box Hardware
Below is a list of hardware parts proposed for the black box prototype. Note that the Arduino Mega board is used to allow connecting all sensors to a single board and uses 5V power that is compatible with the sensors. If an equivalent board of another brand is used, it must also use the 5V rating, rather than 3.3V. Additionally the HC-05 bluetooth module is used in place of an HC-06 or alternate unit, to allow functioning as bluetooth master in order to pair with and query the ELM327 plug connected to the vehicle's’ OBD II port. If a different bluetooth module is used, bluetooth AT mode must be supported.

* 1x ELM327 OBDII Bluetooth adapter
* 1x Arduino Mega*
* 1x HC-05 Bluetooth module
* 1x Arduino SD card reader module
* 1x Arduino Neo 6M GPS module
* 20x Male-Female jumper wires
* 1x Arduino HC-SR04 Ultrasonic Sensor
* 1x GPS antenna (with SMA connector)
* 1x Arduino XBee Shield
* 1x UFL Mini to SMA adapter
* 1x SD 8GB card
* 1x Car 5v USB power source

## Schematic and Installation

Programming code for the black box prototype is not final, and is a topic of further research. The following sections outline the wiring and installation of the black box prototype. 

### Arduino Schematic

![obdII_prototype](assets/img/obdII_prototype.png)

### Device Pinouts

Component | Arduino Pin
------------ | -------------
SD Card Reader | CS - 53, SCK - 52, MOSI - 51, MISO - 50, Vcc - 5v, Gnd - Gnd
GPS | TX - 15, RX - 14, Gnd - Gnd, Vcc - 5v
Ultrasonic Sensor | Echo - 9. Trigger - 8, Vcc - 5v, Gnd - Gnd
Bluetooth | TX - 17, RX - 16, Key - 3, Vcc - 19, Gnd - 18
XBee Radio | Dout - 0, Din - 1, Vcc - 5v, Gnd - Gnd

## Connecting the OBD II Reader

Placement of the OBD II port is standardized and can be located  in the driver-side footwell below the steering wheel. Power is provided through the OBD II port and no configuration of the port is required. The ELM327 socket is plug and play.

