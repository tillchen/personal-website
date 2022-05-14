+++
draft = true
date = 2020-05-22T18:52:12+02:00
title = "Computer Networks Notes"
description = "Course notes for Computer Networks at Jacobs University Bremen"
tags = ["Computer Science"]
+++

* [Introduction](#introduction)
  * [Internet Concepts and Design Principles](#internet-concepts-and-design-principles)
  * [Structure and Growth of the Internet](#structure-and-growth-of-the-internet)
  * [Internet Programming with Sockets](#internet-programming-with-sockets)
* [Fundamental Concepts](#fundamental-concepts)
  * [Classification and Terminology](#classification-and-terminology)
  * [Communication Channels and Transmission Media](#communication-channels-and-transmission-media)
  * [Media Access Control](#media-access-control)
  * [Transmission Error Detection](#transmission-error-detection)
  * [Sequence Numbers, Acknowledgements, Timer](#sequence-numbers-acknowledgements-timer)
  * [Flow Control and Congestion Control](#flow-control-and-congestion-control)
  * [Layering and the OSI Reference Model](#layering-and-the-osi-reference-model)
* [Local Area Networks](#local-area-networks)
  * [Local Area Networks Overview](#local-area-networks-overview)
  * [Ethernet](#ethernet)
  * [Bridges](#bridges)
  * [Virtual LAN](#virtual-lan)
  * [Port Access Control](#port-access-control)
  * [Wireless LAN](#wireless-lan)
* [Internet Network Layer](#internet-network-layer)
  * [Concepts and Terminology](#concepts-and-terminology)
  * [IPv6](#ipv6)
  * [IPv4](#ipv4)
* [Internet Routing](#internet-routing)
  * [Distance Vector Routing (RIP)](#distance-vector-routing-rip)
  * [Link State Routing (OSPF)](#link-state-routing-ospf)
  * [Path Vector Policy Routing (BGP)](#path-vector-policy-routing-bgp)
* [Internet Transport Layer (UDP, TCP)](#internet-transport-layer-udp-tcp)
  * [Transport Layer Overview](#transport-layer-overview)
  * [UDP](#udp)
  * [TCP](#tcp)
* [Firewalls and Network Address Translators](#firewalls-and-network-address-translators)
  * [Middleboxes](#middleboxes)
  * [Firewalls](#firewalls)
  * [Network Address Translators](#network-address-translators)
* [Domain Name System (DNS)](#domain-name-system-dns)
  * [Overview and features](#overview-and-features)
  * [Resource Records](#resource-records)
  * [Message Formats](#message-formats)
  * [Security and Dynamic Updates](#security-and-dynamic-updates)
  * [Creative Usage](#creative-usage)
* [Augmented Backus Naur Form (ABNF)](#augmented-backus-naur-form-abnf)
  * [Basics, Rule Names, Terminal Symbols](#basics-rule-names-terminal-symbols)
  * [Operators](#operators)
* [Electronic Mail (SMTP, IMAP)](#electronic-mail-smtp-imap)
  * [Components and Terminology](#components-and-terminology)
  * [Simple Mail Transfer Protocol (SMTP)](#simple-mail-transfer-protocol-smtp)
  * [Multipurpose Internet Mail Extensions (MIME)](#multipurpose-internet-mail-extensions-mime)
  * [Internet Message Access Protocol (IMAP)](#internet-message-access-protocol-imap)
  * [Filtering of Messages (SIEVE)](#filtering-of-messages-sieve)
* [HyperText Transfer Protocol (HTTP)](#hypertext-transfer-protocol-http)
  * [URLs, URNs, URIs, IRIs](#urls-urns-uris-iris)
  * [HTTP 1.1 Methods](#http-11-methods)
  * [HTTP 1.1 Features](#http-11-features)
  * [HTTP 2.0](#http-20)
* [Multimedia over the Internet](#multimedia-over-the-internet)
  * [Voice over IP](#voice-over-ip)
  * [Real-Time Transport Protocol (RTP)](#real-time-transport-protocol-rtp)
  * [Session Initiation Protocol (SIP)](#session-initiation-protocol-sip)
* [References](#references)

## Introduction

This is the course notes for Computer Networks at Jacobs University Bremen.

### Internet Concepts and Design Principles

1. Internet Addresses:
    * Leading nulls in IPv6 addresses can be omitted and two consecutive  colons can represent a sequence of nulls:
      * 2001:00db8:0000:0000:0000:0000:0000:0001 can be written as 2001:db8::1
    * IPv6 addresses have 16 bytes (4 x 8 groups); IPv4 addresses have 4 bytes.

2. Autonomous Systems:
    * An AS is a set of routers and networks under the same administration.
    * IP packets are forwarded between ASs by EGP (Exterior Gateway Protocol); within an AS, it's IGP (Interior Gateway Protocol)
    * The Internet is a collection os ASs. There's no central authority.

### Structure and Growth of the Internet

1. Internet Exchange Points (IXPs) are switching hubs where many Internet Service Providers (operating ASs) connect in order to exchange internet traffic.

### Internet Programming with Sockets

1. Sockets are abstract communication endpoints with a rather small number of associated function calls.

2. Connection-less vs connection-oriented communication
    * Connection-oriented communication requires a establishment and a teardown. Most of the Internet traffic is using this.

## Fundamental Concepts

### Classification and Terminology

1. Communication modes:
    * Unicast - 1:1
    * Multicast - 1:n
    * Concast - n:1
    * Multipeer - m:n
    * Anycast - 1: nearest receiver
    * Broadcast - 1: all
    * Geocast - 1: n in a region

2. Communication protocols define the syntax and semantics of messages.

3. Circuit vs packet switching:
    * Circuit switching: create and remove (virtual) circuit (e.g. the telephone network)
    * Packet switching: data is carried in packets (e.g. Internet)

4. Connection-oriented vs connection-less:
    * Connection-oriented: stateful (e.g. fetching a Web page)
    * Connection-less: stateless (e.g. Internet name lookups)

5. Data vs. control vs. management plane:
    * Data plane: forwarding of data (hardware)
    * Control plane: telling the data plane how to forward data (routers and switches)
    * Management plane: configuration and monitoring of data and control planes (may involve humans)

6. Topologies:
    * Star; Ring; Meshed Network; Bus; Line.

### Communication Channels and Transmission Media

1. Signals are in general modified during transmission, leading to transmission errors.

2. Data rate (bit rate) vs bit time:
    * Bit time is the time needed to transmit a single bit (1 microsecond for 1 Mbit/s)

3. Delay is the time needed to transmit a message from the source to the sink. It consists of transmission delay and propagation delay.

    ![transmission vs propagation delay](/images/transmission_propagation_delay.png)

4. Bit error rate is the probability of a bit being changed during transmission.

5. Simple wires can easily experience crosstalk caused by capacitive coupling.

6. Transmission impairments:
    * Attenuation
    * Delay distortion (different frequencies arrive at different time)
    * Noise

### Media Access Control

1. Media access control - shared transmission media require coordinated access to the medium.

2. Frequency division multiplexing (FDM): simultaneous signals in different frequency bands.

3. Wavelength division multiplexing (WDM): different wavelengths at the same time.

4. Time division multiplexing (TDM): signals are assigned to specific time slots.

5. Pure aloha - sends data as soon as data becomes available (not very efficient)

6. Slotted aloha - sends do not send immediately but wait for the beginning of a time slot - (slightly more efficient)

7. Carrier sense multiple access (CSMA) - sense the media whether it's unused before starting a transmission.

8. CSMA with collision detection (CSMA-CD) - terminates the transmission as soon as a collision has been detected (and retries after some random delay).

9. Multiple access with collision avoidance (MACA) - sends RTS (ready to send) and CTS (clear to send)

10. Token passing - a token is a special bit pattern circulating between stations (only the station holding the token is allowed to send data).

### Transmission Error Detection

1. Simple parity bits can be added to code words to detect bit errors.

2. Cyclic redundancy check (CRC) uses polynomials (1101 = $x^3+x^2+1$)
    * Divide the message with the generator, if the remainder is 0, we assume no transmission error. Otherwise, add the remainder to the message to get the real bit sequence.

### Sequence Numbers, Acknowledgements, Timer

1. Errors:
    * Bit errors
    * Loss of complete data frames
    * Duplication of complete data frames
    * Receipt of data frames that were never sent
    * Reordering of data frames during transmission

2. End-to-end flow control - the sender must adapt its speed to the speed of the receiver.

3. Congestion control - the sender must react to congestions.

4. The sender assigns growing sequence numbers to all data frames to deal with the errors above.

5. The receiver sends ACK to handle errors.
    * ACK: positive acknowledgement
    * NACK: negative acknowledgement
    * Stop-and-wait protocol - a frame is only transmitted if the previous frame was acknowledged.

6. A send can also use a timer to retransmit a frame if no ACK has been received in time.

### Flow Control and Congestion Control

1. Flow control - allows the sender to send multiple frames before waiting for ACKs.

2. Sliding window flow control - the size of the window and the speed of the sender must match the buffer capacity of the receiver.
    * For the send: LFS - LAR + 1 <= SWS (last frame sent, last ACK received, send window size)
    * For the receiver: LFA - NFE + 1 <= RWS (last frame acceptable, next frame expected, receive window size)

3. Congestions control is used to adapt the speed of the sender to the speed of the network.

### Layering and the OSI Reference Model

1. Protocol data units (PDUs) are exchanged between peer entities; service data units (SDUs) are exchanged between layers (services).

2. The Open systems interconnection model (OSI model): (layer 7 -> layer 1)
    * Application
    * Presentation - data compression/integrity services etc.
    * Session - security services
    * Transport - communication channels
    * Network - determination of paths
    * Data link - transmission of bit sequences in so called frames
    * Physical - transmission of an unstructured bit stream

## Local Area Networks

### Local Area Networks Overview

1. IEEE 802 addresses (MAC addresses) are 6 octets (48 bits) long.
    * It's usually separated using colons or hyphens (00:D0:59:5C:03:8A)
    * The highest bit indicates whether it's unicast (0) or multicast (1)
    * The broadcast address is FF-FF-FF-FF-FF

### Ethernet

1. Classic Ethernet usd CSMA/CD and shared bus.

2. Today's Ethernet uses a star topology with full duplex links.

### Bridges

1. Source routing bridges - sender routes the frame through the bridged network.

2. Transparent bridges - bridges are transparent to senders and receivers.
    * Look up an entry in the forwarding DB (entry added when a frame is received) and forward the frame to the port.
    * If not matching entry exists, do flooding - forward the frame to all outgoing ports except the port from which the frame was received.

3. Port states:
    * Blocking
    * Listening
    * Learning
    * Forwarding
    * Disabled

4. A bridged LAN has a single broadcast domain - frames sent to this address will be forwarded on all links.

### Virtual LAN

1. VLANs provide a separation of logical LAN topologies from physical LAN topologies, which separates the traffic and reduces the network load.

### Port Access Control

1. Port-based network access control grants access to a switch port based on the identity of the connected machine.

2. Components:
    * Supplicant - on a machine
    * Authenticator - on a bridge
    * Authentication server

### Wireless LAN

1. Data frames - "useful" payloads

2. Control frames - facilitates the exchange of data frames
    * RTS & CTS
    * ACK

3. Management frames - maintenance of the network

## Internet Network Layer

The Internet network layer provides a packet-oriented connection-less data exchange function.

### Concepts and Terminology

1. Terminology:
    * Node - a device which implements an Internet Protocol.
    * Router - a node that forwards IP packets not addressed to itself.
    * Host - any node that's not a router.
    * Link - a communication channel.
    * Neighbors - the set of all nodes attached to the same link.
    * Interface - a node's attachment to a link.
    * IP address - identifies an interface or set of interfaces.
    * IP prefix - the initial part of an IP address identifying an IP network.
    * IP packet - a bit sequence of an IP header and the payload.
    * Link MTU - maximum transmission unit (max packet size) on a link.
    * Path MTU - min link MTU of all the links in a path.

2. Calculate the available addresses:
    * IPv4 - $2^{32-prefix} - 2$
    * IPv6 - $2^{128-prefix} - 2$

3. we use longest prefix match for IP forwarding in a forwarding table.

### IPv6

1. Error handling - ICMPv6 (Internet Control Message Protocol Version 6).

### IPv4

1. Error handling - ICMPv4.

2. IPv4 fragmentation - IPv4 packets that do not fit the link MTU will get fragmented into smaller packets. It is considered harmful.

3. Dynamic Host Configuration Protocol (DHCP) allows nodes to retrieve configuration parameters from a central configuration server.

## Internet Routing

### Distance Vector Routing (RIP)

1. Routing Information Protocol (RIP) is a simple distance vector routing protocol that uses Bellman-Ford.

2. Count-to-infinity will cause the costs to reach infinity. (A -- B -- C, A--B breaks, B -> C -> A, C updates the hop count)

3. Split horizon - nodes never announce the reachability. Doesn't solve count-to-infinity.

4. Split horizon with poisoned reverse - nodes announce the unreachablility to neighbors. - Doesn't solve count-to-infinity for all cases since the exchange of distance vectors is not synchronized to a global clock.

5. RIP runs on UDP.

### Link State Routing (OSPF)

1. Open Shortest Path First (OSPF) is a link state routing protocol that uses Dijstra.

2. It's used for IS-IS (intermediate system to intermediate system).

3. Router classification:
    * Internal
    * Area Border
    * Backbone
    * AS Boundary

4. Stub areas are areas with a single area boarder router.

### Path Vector Policy Routing (BGP)

1. Border Gateway Protocol (BGP) is a path vector policy routing protocol that propagates reachability info between ASs.

2. AS categories:
    * Stub - only one peering relationship with one other AS. It only carries local traffic.
    * Multihomed - peering relationships with more than one other ASs. It refuses to carry transit traffic.
    * Transit - peering relationships with more than one other ASs. It's designed to carry both local and transit traffic.

3. BGP doesn't suffer from count-to-infinity. The AS path info allows to detect loops.

## Internet Transport Layer (UDP, TCP)

### Transport Layer Overview

1. The transport layer provides communication services for apps running on hosts.

2. Network layer addresses identify interfaces on nodes (node-to-node significance); Transport layer addresses identify communicating app processes (end-to-end significance).

3. UDP provides a simple unreliable best-effort datagram service.

4. TCP provides a bidirectional, connection-oriented and reliable data stream.

5. Stream Control Transmission Protocol (SCTP) and Datagram Congestion Control Protocol (DCCP).

### UDP

1. UDP datagrams can be multicasted to a group of receivers.

### TCP

1. Header fields:
    * Sequence Number: the sequence number of the first data byte in the segment.
    * ACK: the next sequence number which the sender of the ACK expects.
    * Window: the number of data bytes the sender is willing to receive (the window starts with the ACK).

2. Flags:
    * ACK - indicates the ACK number filed is significant
    * SYN - synchronization of sequence numbers
    * FIN - no more data from sender

3. Connection establishment:
    * Handshake protocol establishes the connection
    * Guarantees correct connection. even if packets are lost or duplicated.

4. Connection Tear-down - bidirectional

5. Flow control - both TCP engines advertise the buffer sizes during connection establishment.

6. TCP congestion control:
    * Fundamentally important to avoid a collapse of the Internet.
    * Congestion window (cwnd) defines how much data can be in transit.
      * It's maintained by a TCP sender in addition to the flow control receiver window (rwnd).
    * The sender uses the two windows to limit the flightsize (the data sent but not yet received) to the minimum of the windows.

7. Retransmission timer controls when a segment is resent if no ACK has been received.

## Firewalls and Network Address Translators

### Middleboxes

1. A middlebox ia any intermediary device performing functions other than the normal standard functions of an IP router.

2. It challenges the End-to-End principle.

3. Types of middleboxes:
    * Network Address Translators (NAT): a function that dynamically assigns a globally unique address to a host.
    * IP Firewalls
    * Proxies
    * ...

### Firewalls

1. Firewall is a system that enforces access control policies between networks.

2. Conservative firewalls allow known desired traffic and reject everything else.

3. Optimistic firewalls reject known unwanted traffic and allow the rest.

4. Firewalls typically consist of packet filters, transport gateways, and application level gateways.

5. Firewall architectures:
    * Screening router: the simplest - with a packet filter.
    * Bastion host: a multihomed host which doesn't forward IP datagrams. but instead provide suitable gateways. - prevents direction host communication.
    * Two packet filters which create a demilitarized zone (DMZ) - most common.
      * Externally visible servers and gateways are in the DMZ.

### Network Address Translators

1. Basic NAT: translates private IP address to public IP address.

2. Network Address Port Translation (NAPT): translates transport endpoint identifiers. It allows to share a single public address among many private addresses (masquerading).

3. Full cone NAT is a NAT where all requests from the same internal IP address and port are mapped to the same external IP address and port.
    * Any external host can send a packet to the internal host by sending a packet to the mapped external address.

4. Restrict cone NAT: unlike full cone, an external host (with IP address X) can send a packet to the internal post only if the internal host had previously sent a packet to IP address X.

5. Port restricted cone NAT also includes the restriction for the port number.

6. Symmetric NAT: only the external host that receives a packet can send a UDP packet back to the internal host.

## Domain Name System (DNS)

### Overview and features

1. DNS provides a global infrastructure to map human friendly domain names into addresses. - name resolution.

2. The resolver is typically tightly integrated into the OS or more precisely the standard libraries.

3. Most hosts do not resolve names themselves but instead the resolver is sending recursive queries to a domain name resolver.

4. Administration of the name space can be delegated.

5. The original DNS protocol doesn't provide sufficient security. There's no reason to trust DNS responses.

6. DNS labels are case-insensitive.

7. The absolute paths ending at the virtual root node end with a trailing dot.

### Resource Records

1. Resource Records (RRs) hold typed info for a given name with the components:
    * Owner - domain name
    * Type - A(IPv4 address)/AAAA(IPv6 address)/CNAME(Canonical Name)/HINFO(Host Info)/MX(Mail Exchanger)/NS(authoritative server)/PTR(pointer to another part of the name space)/SOA(Start Of zone of Authority)
    * Class - IN for Internet
    * Time to Life (TTL) - how many second info can be stored in a local cache (how long the response record is valid)
    * Data Format (RDATA) - depends on the type

### Message Formats

1. Protocol header + questions + answers (RRs) + pointers to authorities + additional info.

2. Simple DNS queries usually use UDP - low overhead, which is important for resolvers that may need to contact many DNS servers.

3. For large data transfers, DNS may utilize TCP.

### Security and Dynamic Updates

1. Resource Record Signature (RRSIG) RR stores digital signatures.

### Creative Usage

1. DNS Blacklists (email spam with A and TXT)

## Augmented Backus Naur Form (ABNF)

### Basics, Rule Names, Terminal Symbols

1. A text-based encoding of protocol messages are programmer readable byt less efficient.

2. ABNF is used to formally specify textual protocol messages. It consists of set of rules (productions)

3. Comments start with ;.

4. The name of a rule must start with an alphabetic character.

5. Terminal symbols are non-negative numbers. We can use . as concatenation and - as a value range operator. They can also be defined by ASCII characters in double quotes.

6. %s - case sensitive; %i - case insensitive (the default).

### Operators

1. Concatenation: empty word

2. Alternatives: /

3. Grouping: ()

4. Repetitions: n*m (n and m are optional)
    * n is the min (default 0) and m (default inf) is the max for repetitions

5. Optional: []
    * [ab] = *1(ab)

## Electronic Mail (SMTP, IMAP)

### Components and Terminology

1. Terminology:
    * Mail User Agent (MUA) - source or target
    * Mail Transfer Agent (MTA) - server and clients that transport
    * Mail Delivery Agent (MDA) - delivers to the mailbox

### Simple Mail Transfer Protocol (SMTP)

1. It's running over TCP.

### Multipurpose Internet Mail Extensions (MIME)

1. MIME supports multiple different char sets, different media types, etc.

2. MIME is widely implemented and not only for mail.

3. MIME boundaries: a delimiter consist of two dashes followed by the boundary.

### Internet Message Access Protocol (IMAP)

1. IMAP allows a client to access and manipulate mail messages stored on a server.

2. It's on TCP.

3. It's strongly suggested to use TLS to encrypt.

4. IMAP supports asynchronous, concurrent operations.

### Filtering of Messages (SIEVE)

1. SIEVE can either be implemented on the client or the server.

## HyperText Transfer Protocol (HTTP)

1. HTTP runs on TCP.

### URLs, URNs, URIs, IRIs

1. URI - Uniform Resource Identifier

2. URL - Uniform Resource Locator
    * A subset of URIs

3. URN - Uniform Resource Name
    * A subset of URIs wih globally unique and persistent name.

4. IRI - Internationalized Resource Identifier

### HTTP 1.1 Methods

1. Methods:
    * GET - retrieve a resource
    * HEAD - retrieve meta-info
    * POST - annotate an existing resource by passing info to it
    * PUT - store info (may create a new resource)
    * DELETE - delete the resource
    * OPTIONS - request info about methods supported
    * TRACE - loopback for testing
    * CONNECT - init a TLS/SSL tunnel

2. Safe methods
    * Safe methods are intended only for information retrieval and should not change the state of the server.
    * They should have no side effects beyond logging
    * GET, HEAD, OPTIONS, TRACE

3. Idempotent methods
    * Idempotent methods can be executed multiple times without producing results that are different from a single execution.
    * PUT, DELETE
    * POST, CONNECT are not idempotent.

4. Supporting caches well is a fundamental goal of HTTP.

### HTTP 1.1 Features

1. Persistent connections and pipelining. (Make multiple requests without waiting for each response.)

2. Chunked transfer encoding.

3. Caching and proxies - most interesting and complex part of HTTP.

4. Negotiation

5. Conditional requests

6. Entity tags (ETag) is an opaque identifier assigned by a web server to a specific version of resource found at a URL.

### HTTP 2.0

1. HTTP/1.0 - separate TCP connection for every resource request

2. HTTP/1.1 - persistent connections and pipelining & conditional requests

3. HTTP/2 - header compression & multiplexing & server push into client caches

## Multimedia over the Internet

### Voice over IP

1. UDP can be used.

### Real-Time Transport Protocol (RTP)

1. End-to-end real-time data.

2. Commonly used over UDP.

### Session Initiation Protocol (SIP)

1. Create, modify, and terminate sessions with one or more participants.

## References

* <https://cnds.jacobs-university.de/courses/cn-2019/>
