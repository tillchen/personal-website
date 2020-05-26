---
title: "Secure and Dependable Systems Notes"
date: 2020-05-26T16:11:13+02:00
draft: false
description: "Course notes for Secure and Dependable Systems at Jacobs University Bremen"
tags: ["Computer Science"]
---

* [Introduction](#introduction)
  * [Recent Computing Disasters](#recent-computing-disasters)
  * [Dependability Concepts and Terminology](#dependability-concepts-and-terminology)
* [Software Engineering Aspects](#software-engineering-aspects)
* [Software Vulnerabilities and Exploits](#software-vulnerabilities-and-exploits)
* [Cryptography](#cryptography)
* [Secure Communication Protocols](#secure-communication-protocols)
* [Information Hiding and Privacy](#information-hiding-and-privacy)
* [System Security](#system-security)
* [References](#references)

## Introduction

This is the course notes for Secure and Dependable Systems by Dr. Jürgen Schönwälder at Jacobs University Bremen.

### Recent Computing Disasters

1. Main memory and CPU memory caches: when the data need is in the main memory but not in the caches, the CPU has to wait quite a while.

2. Side channel attack: an attach where info is gained fro the physical implementation of a computer system (e.g., timing, power consumption, radiation) rather than weaknesses in an implemented algorithm itself.

3. Speculative execution: in a situation where a CPU would have to wait for slow memory, simply guess a value and continue execution speculatively.

4. Reading arbitrary memory

### Dependability Concepts and Terminology

1. System, environment and system boundary:
    * System: an entity that interacts with other entities (e.g. OS kernel)
    * Environment: the other systems. (e.g. hardware)
    * System boundary: the common frontier between the system and its environment. (e.g. the set of OS calls)
    * Systems almost never exist in isolation.

2. Components and state:
    * Components: the structure of a system is composed of a set of components, where each component is another system. The recursion stops when a component is considered atomic.
    * Total state: the set of the following states: computation, communication, stored info, interconnection, and physical condition.

3. Function and behavior:
    * Function: what the system is intended to do, which is described by the function specification.
    * Behavior: what the system does to implement its function and is described by a sequence of states.

4. Service and correct service:
    * Service: the behavior perceived by users; a user is another system that receives service from the service provider.
    * Correct service: the service that implements the system function.

## Software Engineering Aspects

## Software Vulnerabilities and Exploits

## Cryptography

## Secure Communication Protocols

## Information Hiding and Privacy

## System Security

## References

* <https://cnds.jacobs-university.de/courses/sads-2020/>
