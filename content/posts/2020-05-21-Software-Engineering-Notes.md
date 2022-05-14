+++
draft = true
date = 2020-05-21
title = "Software Engineering Notes"
description = "This is the course notes for Software Engineering at Jacobs University Bremen."
tags = ["Software Engineering"]
+++

* [Introduction](#introduction)
  * [Intro](#intro)
  * [Socio-Technical Systems](#socio-technical-systems)
* [The Software Lifecycle](#the-software-lifecycle)
  * [Software Lifecycle](#software-lifecycle)
  * [Requirements Engineering](#requirements-engineering)
  * [UML](#uml)
  * [Design Patterns](#design-patterns)
  * [Compiling and Linking](#compiling-and-linking)
  * [Defensive Programming](#defensive-programming)
  * [Configuration, Version, and Release Management](#configuration-version-and-release-management)
  * [Software Testing](#software-testing)
* [Web and Other Applications](#web-and-other-applications)
  * [Application Architectures](#application-architectures)
  * [GUI Technology](#gui-technology)
  * [Web-Enabled Information Systems](#web-enabled-information-systems)
  * [UI Design](#ui-design)
  * [Web Design](#web-design)
* [Project and Process Management](#project-and-process-management)
  * [Project Management](#project-management)
  * [Software Process Models](#software-process-models)
* [Security](#security)

## Introduction

This is the course notes for Software Engineering at Jacobs University Bremen.

### Intro

1. The code-and-fix cycle:
    * Maintainability and reliability decrease continuously (entropy).
    * If the programmer leaves, all know-how leaves.
    * If the developer is not the user, we get frequent dissent about expectations vs implementations.

2. Common problems:
    * Complexity
    * Integration requirements
    * Quality requirements
    * Flexibility requirements
    * Portability and internationalization requirements
    * Organizational requirements
      * Communication problems/ bad project management

3. Software engineering is multi-person construction of multi-version software

4. Software = programs + documentation

5. System engineering = hardware + software + process engineering

6. Good software delivers functionality and performance that is (MEDA)
    * Maintainable
    * Efficient
    * Dependable
    * Acceptable

### Socio-Technical Systems

1. System = software + hardware + people

2. System categories:
    * Technical = software + hardware
    * Socio-technical = technical systems + operational processes & people

3. Socio-technical system characteristics:
    * Emergent properties
    * Non-deterministic
      * Partially dependent on human operators + a time-varying environment
    * Complex relationships with organizational objectives

## The Software Lifecycle

### Software Lifecycle

1. Requirements Engineering
2. Design
3. Coding
4. Verification & Testing
5. Deployment & Maintenance

### Requirements Engineering

1. Requirements engineering = services + constraints

2. Types of requirements:
    * User requirements - understandable by users (UML)
    * System requirements

3. Another way of classification:
    * Functional requirements
    * Non-functional requirements - properties + constraints
    * Domain requirements

### UML

1. Use case diagrams
    * Use case = a chunk of functionality
    * Actor = someone/something that interacts with the system

2. Activity diagrams
    * Represents the overall flow of control
    * User-perceived actions

3. Class diagrams: <https://creately.com/blog/diagrams/class-diagram-relationships/>
    * Relationships:
      * Association - connection
      * Aggregation - stronger: HAS-A
      * Dependency - weaker
    * Arrow head means it's traversable only this direction

4. Sequence diagrams: object interactions in a time sequence
    * Actors + system components (unlike activity diagrams)

5. State transition diagrams: lifecycle of a given class

6. Alternative: DSLs (domain specific modelling languages)
    * Better used for embedded systems

### Design Patterns

1. Pattern = description + essence of its solution

2. Design pattern = re-using abstract knowledge; generic, reusable design templates for OOP

3. Singleton: ensure a class has only one instance and provide a global access point

4. Observer: separate the display of object state from the object itself

5. Mediator: define an object that encapsulates how a set of objects interact
    * Loose coupling

6. Facade: a unified interface to a set of interfaces in a subsystem
    * Loose coupling

7. Proxy: a surrogate or placeholder for another object to control access to it

8. Adapter: let classes work together that could not otherwise because of incompatible interfaces

9. Composite: compose objects into tree structures to represent part-whole hierarchies

10. Types:
    * Creational
    * Structural
    * Behavioral

### Compiling and Linking

1. source + header files -> Preprocessor -> source -> compiler -> object code -> linker -> executable

2. Preprocessor: textual substitution (with directives #)

3. Object files contain code for a program fragment.

4. Name mangling (name decoration): compiler modifies names to make them unique

5. Linker generates one executable from several object and library files.

6. Strip:
    * By default, executables contain symbol tables which allows reverse engineering and makes the code files substantially larger.
    * Strip executables before shipping

7. Library = archive file containing a collection of object files
    * Object files are linked in completely, from library only what is actually needed

### Defensive Programming

1. Defensive programming intends to ensure the continuing function of the software despite the unforeseeable usage. - Defend against errors (avoid bugs)

2. Invariants:
    * Loop invariants
    * Class invariants: true before and after each method call
    * Method invariants: meet the pre and post conditions - part of design-by-contract (methods are contracts with the user)
    * Ways of enforcing invariants
      * Assertions
      * Exceptions
      * Return codes

3. Structured programming: only use a small set of programming constructs
    * Good: sequence, condition, repetition
    * Bad: goto, break, continue

4. Code guide = uniform style + best practice

### Configuration, Version, and Release Management

1. Software configuration management (SCM) is the discipline of controlling the evolution of software systems.

2. Three classic problems:
    * The double maintenance problem
    * The shared data problem
    * The simultaneous update problem

3. A delta is a difference between two revisions.

4. Terminology:
    * Release: a version that's available to user/client
    * Configuration: combination of components into a system according to case-specific criteria
    * Baseline: a static reference point

5. Configuration models:
    * Composition model = set of objects -- module oriented
    * Change set model = bundle of changes -- dynamic
    * Long transaction model = all changes all isolated into transactions -- collaborative

### Software Testing

1. Software testing: find errors before delivering to the end user.

2. Unit testing: (code)
    * Test driver = dummy environment
    * Test stub = dummy methods
    * Equivalence class testing: build equivalence classes and test one candidate per class.

3. Integration testing: (design)
    * Test interactions among units (e.g. type compatibility)

4. System testing: (requirements)
    * Determine whether system meets requirements
    * Focus on use & interaction
    * Alpha & Beta testing

5. Acceptance testing: (users' need)
    * Get approval from customers

6. Testing methods:
    * Static testing - without executing the software
      * Static analysis = control flow + data flow analysis
      * Formal verification
    * Dynamic testing - with executing the software
      * Black-box testing: spec based
      * White-box testing: look inside to check all statements & conditions have been executed at least once
      * Coverage analysis: measures how much of the code has been exercised
        * Statement vs decision vs path coverage
        * Independent paths V(G) = number of simple decisions + 1 or number of enclosed areas + 1
        * C0 = every instruction; C1 = every branch; C2, C3 - every condition once true once false; C4 path coverage; Rule of thumb: 95% C0, 70% C1
      * Memory leaks:
          * Stack: automatic management
          * Heap: explicit allocation and deallocation
      * Performance profiling: benchmark execution to understand where time is being spent
    * Regression testing: run tests and compare the output to same tests on the previous code version

## Web and Other Applications

### Application Architectures

1. Application Types
    * Data processing
    * Transaction processing
    * Event processing
    * Language processing

### GUI Technology

1. Sequential programs

2. Modern GUI:
    * Event-driven: the program waits on the user instead of the other way around
      * All events go to a single event queue
    * Widgets (window gadget): reusable interactive object
      * Handle events
      * Update appearance
      * Generate new events
      * Send to listeners (custom code)
    * Interactor Tree: decompose interactive objects into a tree

3. Model-View-Controller:
    * Model = information the app is manipulating (representation of real world objects)
    * View = a visual representation of the model
      * View will be notified if there are changes for the model
    * Controller:
      * Receives input events
      * Decides what to do
        * Communicates with the view to select the objects
        * Calls model methods to make changes
    * Why?
      * Combining MVC into one class will not scale
      * Separation eases maintenance

### Web-Enabled Information Systems

1. Three-Tier Architecture:
    * Presentation tier
    * Middle tier
    * Data management tier

### UI Design

1. UI should match skills, experience, and expectations of its anticipated users.

2. The human factors;:
    * Limited short-term memory
    * People make mistakes
    * People are different
    * People have different interaction preferences

3. Pressman's Golden Rules:
    * Place user in control
    * Reduce user's memory load
    * Make interface consistent

4. UI design involves:
    * User analysis
    * System prototyping
    * Prototype evaluation

### Web Design

1. Principles:
    * Anticipation
    * Communication
    * Consistency
    * Controlled autonomy
    * Efficiency

## Project and Process Management

### Project Management

1. Activity organization:
    * Activities should produce tangible outputs at well-defined points
    * Milestones: end-point of a process activity
    * Deliverables: projects results
    * Rules:
      * Design tasks as self-contained units with clear goals
      * Concurrent tasks
      * Minimize dependencies

2. Task & Activity Flow Chart = Project Evaluation and Review Technique (PERT chart) - shows relationships between activities.

3. PMs do planning, estimating, and scheduling.

### Software Process Models

1. Waterfall Model:
    * Partitioning into distinct stages - inflexible
    * Few business systems have stable requirements
    * Only appropriate when requirements are well-understood and fairly stable

2. Incremental Methods:
    * Break into increments
    * User requirements are prioritized
    * The requirements are frozen once the increment is started
    * Early increments act as a prototype which helps elicit requirements for later increments
    * Lower risk of failure

3. Agile/XP Methods, Scrum
    * The agile manifesto: value
      * individuals and interactions over processes and tools
      * working software over comprehensive documentation
      * customer collaboration over contract negotiation
      * responding to change over following a plan
    * Principles: (CIPCS)
      * Customer involvement
      * Incremental delivery
      * People, not process
      * Embrace change
      * Maintain simplicity
    * Extreme programming: very small increments (constant code improvement (refactoring))

4. Iterative/Spiral Methods:
    * Loop = one phase
    * No fixed phases

5. Evolutionary Development:
    * Evolve final system from initial outline spec
    * Throw-away prototyping - start with poorly understood requirements
    * Good for well isolated parts (UI)
    * Lack of process visibility
    * Often poorly structured

6. Use incremental dev for best understood requirements and throw-away prototyping for poorly understood.

7. Capability Maturity Model Integration:
    * CMM Levels:
      * Initial
      * Managed
      * Defined
      * Quantitatively Managed
      * Optimizing
    * CMMI Components:
      * 24 process areas
      * Goals
      * Practices

## Security

1. Motivation (objectives):
    * Secrecy - users should not be able to *see* things they are not supposed to
    * Integrity - users should not be able to *modify* things they are not supposed to
    * Availability - users should be able to *see and modify* things they are allowed to

2. 3P security management: process; people; probing your defences
