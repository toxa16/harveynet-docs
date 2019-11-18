# HarveyNet Authorization Flow

## Introduction

As we can see from the system architecture [deployment diagram](./system-architecture-deployment-diagram.png), the HarveyNet architecture consists of many nodes. All the server node need to be protected by an authorization mechanism, while portal nodes will act as authorization clients. Hence, the HarveyNet system will need a unified authorization flow, which can be spread among all architecture nodes.

Such authorization flow is called [OAuth 2.0](https://tools.ietf.org/html/rfc6749). And to implement the Authorization Server we can use a third-party service called [Auth0](https://auth0.com/).

## Current Implementation

While OAuth 2.0 indeed offers a secure flow for distributed systems, and the Auth0 service can greatly simplify the Authorization Server implementation, it is still required to do a lot of work on other architecture nodes to protect them with the OAuth 2.0 flow. Moreover, while implementing the authorization flow we will need to take important decisions, including on conceptual level (I guess).

On the other hand, authorization is not a core functionality of the HarveyNet system. Therefore, to boost initial stages of development I decided **not to perform any authorization-related tasks for now**, focusing on core functionalities instead.

However, we will still need to identify the HarveyNet users and machines (simulators) even for basic functionalities. So, for this I decided to implement a **URL-query based identification flow** (e.g. adding to URLs `?username=<USER_NAME>`).

Of course, this flow can not be considered as a security layer for the system. But it is a simple and robust identification mechanism that will back the core functionalities for us on early development stages.