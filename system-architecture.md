# System Architecture

According to the [concept](./project-concept.md) of the HarveyNet project, there will be two primary portals in the system architecture: control panel web application and hardware controller. The concept also says that there will be other service nodes to provide core functionalities of the entire system. Let's dive into the architecture nodes as they emerge from the project needs.

- [Control Panel](#control-panel)
- [Machine Simulator](#machine-simulator)
- [Control Server](#control-server)
- [Authorization Server](#authorization-server)
- [Ownership Server & Storage](#ownership-server-&-storage)
- [STUN/TURN Server](#stunturn-server)

## Control Panel

The control panel web application (in further reading **Control Panel**) is a portal directly accessible to end users who will own and/or control a Harvey. The Control Panel will provide access to all functionalities about controlling and monitoring a machine.

To be easily accessible for end users, the Control Panel will technically be a dynamic web application (aka single page application or SPA) hosted on a remote server and available via an HTTP URL-address on devices like PCs or tablets.

The Control Panel instances will therefore be run on end users' devices (more precisely, in web browsers). Thus, there may be multiple active Control Panel instances within single deployment of the HarveyNet system at a single moment of time.

As a primary development tool for creating the Control Panel application will be chosen the [React.js](https://reactjs.org/) library. In perspective, due to dynamic nature of the Control Panel, it can be implemented as a progressive web application (PWA) or native mobile application.

## Machine Simulator

The hardware controller device is intended to establish network connection with the HarvetNet system for a Harvey unit and to perform direct control and monitoring of the machine.

However, since the machine controller is a hardware device, its implementation goes beyond the scope of the HarveyNet project. What is still significant within the project is an interface (API) the controller communicates with the control system through.

Thus, it is needed to substitute the hardware controller with a simulated software unit which will be fully compliant with mentioned API and thus will play a role of a self-sufficient system architecture portal.

This portal substitute will be called **Machine Simulator**. It will be used for testing the HarveyNet software system on all levels.

The Machine Simulator will allow its end users (HarveyNet developers and testers) to track the control process of a machine and mock its dynamic behavior.

The Machine Simulator will be created in a form of dynamic web application using React.js, similarly to the Control Panel.

On a single HarveyNet system deployment there may be multiple active instances of the Machine Simulator, thus imitating multiple Harveys simultaneously connected to the HarveyNet system.

## Control Server

To allow control panels and machines (simulators) to find and communicate with each other, some central entity is needed within the HarveyNet system. It will be a service node of the system architecture, used to connect portal instances into single network. It is intended to transmit primarily controlling and monitoring signals between portal instances. For this reason, this node will be called **Control Server**.

The process of controlling and monitoring a Harvey has a real-time nature: it consists of continuous controlling signals and dynamically changing parameter values. Thus, the Control Server will be a websocket server, providing websocket APIs for the portals. This way real-time control process can be implemented within the HarveyNet system.

The Control Server will be developed on [Node.js](https://nodejs.org/) platform, because it is a great tool for developing real-time web applications.

The Control Server will be deployed on a remote server and available online from some URL-address for portal instances. Hence, within single deployment of the HarveyNet system there will always be only one Control Server instance at any moment of time.

## Authorization server

As we said above, there may be multiple Control Panel instances and machine (Machine Simulator) instances within a single deployment of the HarveyNet system. Machine operators must be able to access only the machines they own. Moreover, we need to provide for the Control Server a security mechanism to allow establishing websocket connections only for valid clients. Hence the HarveyNet system needs *user authorization* functionality.

The HarveyNet system is a distributed software system, which consists of multiple architecture nodes. So we will implement an authorization scheme of the system using the [OAuth 2.0 authorization framework](https://tools.ietf.org/html/rfc6749). The OAuth 2.0 framework provides great tools for implementing a unified authorization scheme of a software system spread between its architecture nodes.

The OAuth 2.0 framework includes several so-called authorization roles (client, resource server, authorization server etc), and each of the HarveyNet system nodes is playing one of these roles. In particular, the Control Panel and the Machine Simulator are *clients* in the OAuth 2.0 flow, and the Control Server is a *resource server*.

The HarveyNet system needs a dedicated service node which will perform core functionalities of the OAuth 2.0 flow. This service node will be called **Authorization Server**, and this will be the only node of the system architecture playing the OAuth 2.0 authorization role of the same name.

While the Authorization Server can be developed from scratch within the HarveyNet project, it is also possible to use a third-party service called [Auth0](https://auth0.com/). This is a platform providing user authorization functionality as a service, including OAuth 2.0 flow usage. The Auth0 offers a free tier, which will be convenient for initial development, and even allows to use its services within various deployment environments, which will be very useful for automated testing.

## Ownership Server & Storage

Every machine operator (the Control Panel user) will own, and respectively have access to certain machine(-s). And while this restricted access will be secured by the system user authorization flow, the HarveyNet system still needs a registry to store and manage data about machine ownerships.

Such registry will be implemented as two service nodes of the system architecture: **Ownership Server** and **Ownership Storage**.

Regardless of conceptual ownership scheme of the system (i.e. how many machines can own a single user) the ownership data is permanent and not frequently changed. Hence, the Ownership Storage will be implemented as a *DBMS* (possibly as a third-party service) which will contain a database to store the ownership data.

The Ownership Server will be the only node of the HarveyNet system which will have direct access to the Ownership Storage. It will provide an HTTP RESTful interface for other nodes to manage the ownership data. The Ownership Server will be protected by the OAuth 2.0 flow as a *resource server*.

The Ownership Server (and hence the Ownership Storage indirectly) will be used mostly by the Control Panel and the Control Server, as these two nodes are going to need this data much more than others.

## STUN/TURN Server

One of the HarveyNet system functionalities is video streaming from machine on-board cameras to a Control Panel dashboard. And since all HarveyNet networking is built on top of HTTP and WebSocket protocols and some architecture nodes are browser-based, then a good choice for video streaming will be WebRTC.

WebRTC establishes a peer-to-peer connection to exchange video and audio data, however, to deal with issues of real network connectivity the **STUN/TURN Server** is needed, which will be deployed as a separate architecture node.

For the HarveyNet system the STUN/TURN Server can be installed, configured and launched using an [open-source package](https://code.google.com/archive/p/rfc5766-turn-server/).

However, there are also third-party providers of STUN/TURN functionalities as a service. They may offer free tiers for convenience of initial development, and are OAuth 2.0 compliant, which is necessary for protecting the STUN/TURN Server as a *resource server* of the HarveyNet system OAuth 2.0 authorization flow.

To allow system client nodes to establish a WebRTC connection the HarveyNet system also needs another architecture node: so-called **Signaling Server**. The Signaling Server is used to exchange media metadata between client nodes, which has to be done before WebRTC connection can be initiated.

The Signaling Server will be implemented as a websocket server, so it may either be deployed as a separate architecture node or integrated into the Control Server. In either way the Signaling Server will be protected by the OAuth 2.0 authorization flow as a *resource server*.

## Deployment Diagram

Figure 1 shows the system architecture [UML](https://www.uml.org/) deployment diagram. The diagram represents all architecture nodes of the HarveyNet system and connections between them within system deployment.

![System Architecture Deployment Diagram](/home/ant/Projects/harveynet/docs/system-architecture-deployment-diagram.png)

**Figure 1. System architecture deployment diagram.**

