---
layout: page
title: Fast-developer feedback
parent: Development practices
nav_order: 3
---

# Who
This practice audience is for developers.

# What
Fast-developer feedback is the practice of local checks before pushing code to a remote repository where the CI/CD pipeline would be triggered.

# Why
In environments with complex or lenghty pipeline steps the feedback to the developer on code linting or security might take longer than 10 minutes. This delay from code push to validation creates a bottleneck for the developers that stops them continue working or publish fixes more quickly.

# Practice
Create a bootstrap repository that developers have to clone and apply to their workstations, the bootstrap will create git hooks to trigger before code commit or push. This steps that take less than a few seconds to complete, must have steps:
- Code linting with mega-linter
- Code security with trivy