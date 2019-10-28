# Project Concept

A sweet potato harvester nicknamed *Harvey* is a modern agricultural machine created to harvest sweet potato vegetables in 21st century. It is designed to function under remote control, including out of range of direct visibility. Such control over Harvey machines will be provided by distributed remote controlling software system, called *HarveyNet*.

The HarveyNet system will allow its end users (Harvey operators) to:

- take control over Harvey's primary functions, such as movement and harvesting;
- monitor machine's parameters (e.g. speed or gasoline level) in real-time mode;
- view streamlined video from machine on-board cameras;
- track current location of a machine via GPS map.

These functionalities will be available to end users via a control panel web application, which will be hosted on a remote server and be accessible online from an HTTP URL.

In turn, on every Harvey unit will be installed some network controlling hardware, which will be connected to HarveyNet via mobile 4G network and perform direct control of the machine according to signals received from HarveyNet. This hardware controller will also read and transmit to the network dynamic machine parameter values and streamline video from on-board cameras.

Implementation details of this hardware controller are beyond the scope of the HarveyNet project. What will matter, however, is the programming interface (API), though which the controller will communicate with the HarveyNet system.

The control panel web application and the hardware controller mentioned above are two end nodes (portals) of the HarveyNet [system architecture](./system-architecture.md). The architecture will also include several other nodes (services and data stores) to power core functionalities such as real-time remote control, video streaming, user (operator) and machine authentication, GPS mapping and so on.

