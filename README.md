# Mesh Networks

The repository is aimed at different methods of creating independent and resilient Ad-Hoc mesh networks.  
The repository will also contain other network related stuff that may not be directly for mesh networks.  
Most of the methods are adoptation/modifications of existing projects/protocols.  

## ESP Module Mesh:
ESP Modules are mainly of two types:  
 1. ESP8266 chip based  
 2. ESP32 chip based  

ESP32 is more powerful and has more features as compared to ESP8266. Mesh implementation using ESPs is aimed at providing a controller-cum-network interface for compact Swarm robots.  
There were intitial efforts to modify ESP modules to make them function as _'Slave'_ network interfaces for superior computers like RasPi. However this has been currently sidelined owing to apparent infeasible implementation methods and an easier way to connect RasPis in a mesh.

## OLSR/Mesh Protocol driven Mesh:
It is possible to modify the properties of wireless interface of Raspberry Pi/ Linux computers to make them compatible for Ad-Hoc mesh networks without additional interfaces.  
Ad-Hoc networks are those which do not depend on existing wireless network infrastructure for connectivity. Just Ad-Hoc supporting network is not sufficent for a mesh network. It also requires a mesh protocol that handles routing in the presence of multiple paths for a given destination. This is provided by protocols like OLSR. OLSR implementation for RasPi mesh is [here]().
