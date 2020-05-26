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
  * [Dependability Metrics](#dependability-metrics)
* [Software Engineering Aspects](#software-engineering-aspects)
  * [General Aspects](#general-aspects)
  * [Software Testing](#software-testing)
  * [Software Specification](#software-specification)
  * [Software Verification](#software-verification)
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

5. Failure, error, and fault: (Threats)
    * Failure: service failure, when the delivered service deviates from correct service.
    * Error: the part of the total state that may lead to subsequent failure.
    * Fault: the adjudged or hypothesized cause of an error. A fault is active when it produces an error, otherwise it's dormant.
    * Fault -> Error -> Failure.

6. Dependability:
    * The ability of a system to deliver service than can justifiably be trusted.
    * The ability to avoid service failures that are more frequent and more severe than is acceptable.

7. Dependability attributes:
    * Availability: readiness to deliver
    * Reliability: continuity
    * Safety: no catastrophic consequences
    * Integrity: no improper alterations
    * Maintainability: undergo modifications
    * Confidentiality: no disclosure of info

8. Security - confidentiality, integrity, and availability.

9. Means:
    * Prevention: preventing the occurence or introduction of faults.
    * Tolerance: avoiding service failures in the presence of faults.
    * Removal: reducing the number and severity of faults.
    * Forecasting: estimating the present number, the future incidence, and the likely consequences of faults.

### Dependability Metrics

1. Reliability and MTTF/MTBF/MTTR:
    * Reliability: the probability that the system is delivering correct service in the time interval.
    * MTTF (Mean Time To Failure): non repairable.
    * MTBF (Mean Time Between Failure): repairable.
    * MTTR (Mean Time To Repair).

2. Availability: the probability that the system is delivering correct service at time t.
    * For a repairable system, A = MTBF/(MTBF + MTTR).
    * 5 nine availability means 99.999%.

3. Safety: the probability that the system is delivering correct service or has failed in the manner that does not cause harm in the interval.
    * Mean Time To Catastrophic Failure.

## Software Engineering Aspects

### General Aspects

1. Defensive programming: requires the preconditions to be checked when a function is called

### Software Testing

1. Unit and regression testing:
    * Regression testing: testing of an entire program to ensure that a modified version of a program still handles all input correctly that an older version did.
    * A bug reported by a customer is primarily a weakness of the regression test suite.

2. Test coverages:
    * Function coverage
    * Statement coverage
    * Branch coverage
    * Predicate coverage (condition coverage): Boolean sub-expression both true and false

3. Mutation testing:
    * Mutation testing involves modifying a program in small ways.
    * It evaluates the effectiveness of a test suite.
    * The source code is modified algorithmically by applying mutation operations in order to produce mutants.
    * A mutant is killed by the test suite if tests fail for the mutant. Mutants that are not killed indicate that the test suite is incomplete.
    * The mutation score is the number of mutants killed normalized by the number of mutants.

4. Fuzzing or fuzz testing feeds invalid, unexpected, or simply random data into computer programs.

5. Fault injection inject faults by:
    * modifying source code (very similar to mutation testing) or
    * injecting faults at runtime (often via modified library calls)
    * It's highly effective to test whether software deals with rare failure situations, e.g. the injection of system calls failures that usually work.

### Software Specification

1. Formal specification: uses a formal (mathematical) notation to provide a precise definition of what a program should do.

2. Formal verification uses logical rules to mathematically prove that a program satisfies a formal specification.

3. Hoare triple: {P}C{Q} (precondition, program, postcondition)

4. Partial correctness and total correctness:
    * Partially correct: the results satisfy the postcondition Q
    * Totally correct: partially correct + always terminates [P]C[Q]

5. Notations:
    * V: variable
    * E: expression
    * S: statement (either true or false)
    * C: command
    * x,y: auxiliary variables

6. Conditionals and while loop:
    * IF S THEN $C_1$ ELSE $C_2$ FI
    * WHILE S DO C OD

### Software Verification

## Software Vulnerabilities and Exploits

## Cryptography

## Secure Communication Protocols

## Information Hiding and Privacy

## System Security

## References

* <https://cnds.jacobs-university.de/courses/sads-2020/>
