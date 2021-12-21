---
layout: page
title: Contract-led development
parent: Development practices
nav_order: 2
---

# What
Contract-led development is the practice of defining the microservices architecture and API interfaces before the code implementation.

# Why
In an environment where you consume a service or provide service yourself it is imperative to have a stable API. Defining the interface before the code implementation helps on understading the interactions between the different subsystems and minimizes future API changes.

# Introduction
Contract-led development establishes a workflow for the architecture and design of the microservices. This approach is as follows:

Domain-driven decoupling -> Contract design -> Microservice implementation