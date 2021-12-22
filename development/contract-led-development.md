---
layout: page
title: Contract-led development
parent: Development practices
nav_order: 1
---

# Who
The audience for this practice is mainly application architects, developers, and CI/CD owners.

# What
Contract-led development is the practice of defining the microservices architecture and API interfaces before the code implementation.

# Why
This contract is used in your CI/CD pipeline for **testing** and **delivery**. In an environment where you consume a service or provide service yourself, it is imperative to have a stable API. Defining the interface before the code implementation helps in understanding the interactions between the different subsystems and minimizes future API changes.

# Introduction
Contract-led development establishes a workflow for the architecture and design of the microservices. This approach is as follows:

Domain-driven decoupling -> Contract design -> Microservice implementation

# Contract artifacts
Contract artifact is any file that describes the API, ideally machine-readable. The most common artifact types are:
- OpenAPI
- AsyncAPI

These artifacts describe the API (contracts) of a service for REST or messaging form. The format of the files usually in JSON or YAML describes the service with comments for the users on how to consume it. The files contain metadata describing the version and lifecycle of the API.

Store these artifacts in a *Git* repository, you can use <example> as guidance.
# Contract versioning
Contracts or API versioning is a requirement for a stable platform, with it you can manage the lifecycle of them and program timelines like end-of-life or deprecations. This becomes critical as deprecating older API versions can shed lines of code to be maintained and reduce toil or technical debt. Newer API versions can be implemented by completely new microservices, like for example in a newer language version, while older versions can still be serviced by their current implementation.