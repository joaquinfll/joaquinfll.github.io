---
layout: page
title: CI/CD
parent: Development practices
nav_order: 2
---

# Who 
This practice audience is for developers or operations teams that maintain a continuous integration or continuous delivery pipeline.

# What 
CI (Continous Integration) is the practice of passing your code through a set of steps to check for code style, quality, security, dependencies, etc.
CD (Continuous deployment or delivery) is the practice of packaging your code to create an artifact that can be consumed, for example in an application server.

# Why 
Developing an application by a group of people is challenging. The code needs to be understood by all the members, maintain the same style, be secure and work. This is archived through the validation of the code in a CI pipeline.

# CI must-have steps 
These steps are a bare minimum of what it should include in your CI:
- **Code and contract linting with mega-linter**: consistency of how the writing style
- **Code security with trivy**: tests the security of code and container dependencies
- **Contract/API compatibility test**: test the compatibility with the current published API version
- **Code build**: how you build your artifact, ex. “go build” and “podman build”

# CD must-have steps 
These steps are a bare minimum of what it should include on your CD:
API versioning: with the results of API compatibility publish the new API with correct versioning
- **Artifact versioning**: your artifact from code build must be versioned
- **Artifact archiving**: your artifact should be published in an artifact repository, ex. Sonatype Nexus
- **API archiving**: your API should be published to a git repository for consumption and documentation

