# Harvey Overview

## General Structure and Functions

A Harvey machine is a robotic unit which consists of two main parts: a carriage vehicle and a working tool installed on it. There may be different tools to operate on the Harvey, e.g. for harvesting, seeding etc. Each tool has its own features and functionalities.

Regardless of a tool installed, the Harvey vehicle part is a wheeled carriage meant for moving. Its hardware is controlled by robotic operating system (ROS). There are several on-board cameras to transmit video to remote controllers. The vehicle is also equipped with a networking module for connecting to remote controlling system (i.e. the HarveyNet).

### Networking module

The networking module is powered by an electric battery and is meant to be constantly turned on (similar to a smartphone or tablet), so the Harvey machine is always connected to the Harveynet and is able to receive controlling signals from remote operators at any moment.

However, there may be occurrences when the networking module is not connected to the HarveyNet. It is possible when the networking unit is manually turned off, or when its battery is exhausted, or when the network connection is lost etc. In such case the Harvey is considered to be disconnected, or offline. Consequently, in the HarveyNet Control Panel the machine operator sees its status "*Offline*", and **all controls for the machine are inactive**.

As soon as the Harvey restores the connection, it is seen in the Control Panel with "*Online*" status, and **some controls become enabled**. (This flow is implemented in [current system prototype](./DEPLOYED-PROTOTYPES.md) v0.0.1 - v0.0.3).

### Main electronics

While the networking module is meant to be always turned on, other electronic equipment of a Harvey doesn't need to be active when the machine is not being in use. It is, for example, on-board cameras, indicators (e.g. for gas level or temperature) or engine starter.

Therefore, in the Control Panel there will be a control (button) which turns on/off these devices. So, after activating main electronics of a Harvey, **it will be possible to start the engine** via a special button.

### Engine

When main electronic devices are turned on, it is now possible to start the engine. The Harvey vehicle has these primary movement functions:

- moving forward and backward;

- steering left and right.

There will be corresponding controls for these functions in the Control Panel, however, **they will be inactive while the engine is stopped**.

After starting the engine, the **movement controls are enabled**.

## MVP Considerations

As a first stable version of the HarveyNet system we need to implement the controlling flow mentioned above, i.e. to implement control over the Harvey's movement and primary electronics. This is the first major development stage of the HarveyNet project with following substages:

- implementation of the Harvey movement control;
- on-board cameras video streaming (as this substage requires implementation of WebRTC, which is not a trivial step).

Upon reaching this development stage it will be possible to test the HarveyNet system on a real Harvey unit.