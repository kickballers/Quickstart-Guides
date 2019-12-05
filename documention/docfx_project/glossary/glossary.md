# Glossary
Here you will find a list of terms you may come across as you are using Ziti.

## Ziti Network, Ziti
Ziti is a modern, programmable network overlay with associated edge components, for application-embedded, zero trust network connectivity, written by developers for developers.

Ziti is NetFoundry's next-generation programmable networking product. Ziti is used to create Ziti Networks.

## Network Overlay, Overlay
A Ziti network is implemented as an "overlay". A network overlay abstracts away the layers beneath it, providing a new set of abstractions for designing and implementing software and systems. Good programming abstractions allow developers to focus on the rules implemented by those abstractions without being concerned with the layers below the abstraction. Ziti's overlay allows developers to focus on connectivity between components without having to be concerned with low-level details of how that connectivity is managed.

## Underlay
We refer to lower-level network concerns as "underlay". IP networking would be an example of an underlay concept.

## Ziti Fabric, Fabric
The Ziti Fabric provides the core of the network overlay. The Ziti Fabric implements a routable mesh network, which can provide reliable connectivity between any two points on the network. The fabric provides software extension mechanisms that allow the overlay to be embedded into new overlay applications. The Ziti Edge is an example of an overlay application implemented on top of Ziti Fabric extension mechanisms (Xgress, Xctrl, Xmgmt).

## Ziti Router, Router
A Ziti Router is a process that is installed on a host, which allows it to participate in a Ziti Fabric. The router is designed to be extensible through Ziti fabric extension mechanisms (Xgress), which means that it is capable of "hosting" overlay network applications like the Ziti Edge.

## Ziti Controller, Controller
A Ziti Controller is a process that is installed on a host, which allows it to coordinate a Ziti network. The controller is designed to be extensible through Ziti fabric extension mechanisms (Xctrl, Xmgmt), which means that it is capable of hosting extensions to the fabric control and management planes.

## Xgress (Xctrl, Xmgmt), Ziti Fabric SDK
Xgress is a set of extension components for the Ziti fabric, which enable overlay applications to participate in the overlay network. Xgress focuses on extending the data plane, providing interfaces for creating initiating and terminating endpoints. Xctrl and Xmgmt focus on extending the control and management planes of the fabric. Xgress is the core of the Ziti Fabric SDK.

## Ziti Edge, Edge
The Ziti Edge implements the zero trust connectivity framework as an overlay application on top of the Ziti Fabric. The Ziti Edge provides connectivity implementations for a number of important endpoint types, including applications that embed Ziti connectivity through the Ziti Edge SDK. The Ziti Edge provides fallback connectivity solutions for non-Ziti applications using components like the Ziti tunnelers, and the Ziti proxy.

## Ziti Endpoint SDK, Endpoint SDK, SDK
The Ziti Endpoint SDK provides software components that are designed to be embedded into customer applications so that they can participate natively in a Ziti network. The SDK targets golang, Swift, C, C#, and potentially other programming languages, allowing programs in those languages to work with idioms and concepts native to those environments. The SDK provides support for both accessing and hosting services that are available on a Ziti network.

## Ziti Enabled Application
A Ziti Enabled Application is an application that embeds the Ziti Endpoint SDK, such that it can participate on a Ziti network to either access or host services.

## Ziti Tunneler, Tunneler
A Ziti Tunneler provides connectivity for applications that are not Ziti enabled. Our tunneler implementations provide an underlay connectivity component (TUN, tproxy, etc.), and then use the Ziti Endpoint SDK such that they can bridge connectivity onto the Ziti network.

## Ziti Service, Service
A Ziti network is primarily concerned with providing access to "services". A service encapsulates the definition of any resource that could be accessed by a client on a traditional network. A Ziti Service is defined by a strong, extensible identity, rather than by an expression of an underlay concept. This means that services defined on a Ziti Network have an almost limitless "namespace" available for identifying services. A Ziti service would be defined by a name and/or a certificate, rather than by a DNS name or an IP address (underlay concepts).

## Service Definition
A service definition is used to "bind" a service to a specific underlay network expression, through one or more nodes on a Ziti overlay network. A service definition usually includes a terminating router (or routers) and one or more SDK or underlay network endpoints where the service can be reached.

## Session
A session is an "instance" of a service on behalf of an initiating endpoint, which is connected to a terminating endpoint. A session has strong identity and security between the initiating endpoint, terminating endpoint, and throughout the links between. A session selects a specific set of routers to traverse between the endpoints, and that path can change dynamically due to network performance.

## Initiating Router, Terminating Router
An initiating router is the router which initiates a request for a session on behalf of a connected endpoint. A terminating router is the router which provides access to the service associated with the session request. Every session links an initiating endpoint (through an initiating router), with a terminating endpoint (through a terminating router).

## Initiating Endpoint, Terminating Endpoint
See "initiating router" and "terminating router" above. The initiating endpoint is the endpoint responsible for requesting connectivity to a service. The terminating endpoint is the endpoint that provides the service.

## Path
The path is the set of Ziti Routers traversed by a session from an initiating router to a terminating router. Ziti aggressively optimizes the path for throughput and reliability, and so it may change during the session.