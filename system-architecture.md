# System Architecture

According to the [concept](./project-concept.md) of the HarveyNet project, there will be two primary portals in the system architecture: control panel web application and hardware controller. The concept also says that there will be other service nodes to provide core functionalities of the entire system. Let's dive into the architecture nodes as they emerge from the project needs.

[TOC]

## Control Panel

The control panel web application (in further reading *Control Panel*) is a portal directly accessible to end users who will own and/or control a Harvey. The Control Panel will provide access to all functionalities about controlling and monitoring a machine.

To be easily accessible for end users, the Control Panel will technically be a dynamic web application (aka single page application or SPA) hosted on a remote server and available via an HTTP URL-address on devices like PCs or tablets.

The Control Panel instances will therefore be run on end users' devices (more precisely, in web browsers). Thus, there may be multiple active Control Panel instances within single deployment of the HarveyNet system at a single moment of time.

As a primary development tool for creating the Control Panel application will be chosen the [React.js](https://reactjs.org/) library. In perspective, due to dynamic nature of the Control Panel, it can be implemented as a progressive web application (PWA) or native mobile application.

## Machine Simulator

The hardware controller device is intended to establish network connection with the HarvetNet system for a Harvey unit and to perform direct control and monitoring of the machine.

However, since the machine controller is a hardware device, its implementation goes beyond the scope of the HarveyNet project. What is still significant within the project is an interface (API) the controller communicates with the control system through.

Thus, it is needed to substitute the hardware controller with a simulated software unit which will be fully compliant with mentioned API and thus will play a role of a self-sufficient system architecture portal.

This portal substitute will be called *Machine Simulator*. It will be used for testing the HarveyNet software system on all levels.

The Machine Simulator will allow its end users (HarveyNet developers and testers) to track the control process of a machine and mock its dynamic behavior.

The Machine Simulator will be created in a form of dynamic web application using React.js, similarly to the Control Panel.

On a single HarveyNet system deployment there may be multiple active instances of the Machine Simulator, thus imitating multiple Harveys simultaneously connected to the HarveyNet system.

## Control Server

To allow control panels and machines (simulators) to find and communicate with each other, some central entity is needed within the HarveyNet system. It will be a service node of the system architecture, used to connect portal instances into single network. It is intended to transmit primarily controlling and monitoring signals between portal instances. For this reason, this node will be called *Control Server*.

The process of controlling and monitoring a Harvey has a real-time nature: it consists of continuous controlling signals and dynamically changing parameter values. Thus, the Control Server will be a websocket server, providing websocket APIs for the portals. This way real-time control process can be implemented within the HarveyNet system.

The Control Server will be developed on [Node.js](https://nodejs.org/) platform, because it is a great tool for developing real-time web applications.

The Control Server will be deployed on a remote server and available online from some URL-address for portal instances. Hence, within single deployment of the HarveyNet system there will always be only one Control Server instance at any moment of time.