# Container (Docker) Hardening 

This is a comprehensive guide to hardening Docker containers. It covers essential security practices, from understanding container risks to implementing advanced security tools. Each module focuses on a specific aspect of container security, helping you build, deploy, and manage containers with confidence.

## Modules Overview

| Module | Title | Details | Description |
|--------|-------|---------|-------------|
| 1 | Introduction to Container Security | [Module 1](module-1.md) | Overview of container security concepts and risks |
| 2 | Securing the Docker Host | [Module-2](module-2.md) | Best practices for securing the Docker host environment |
| 3 | Secure Docker Images | [Module-3](module-3.md) | Techniques for building and maintaining secure images |
| 4 | Secure Container Runtime | [Module-4](module-4.md) | Hardening containers during runtime and managing privileges |
| 5 | Docker Networking and Storage Security | [Module-5](module-5.md) | Securing container networking and storage configurations |
| 6 | Monitoring, Logging, and Compliance | [Module-6](module-6.md) | Implementing monitoring, logging, and compliance controls |
| 7 | Advanced Security Tools | [Module-7](module-7.md) | Using advanced tools for container security and analysis |

## Demos

| Demo | Title | Description | Link |
|------|-------|-------------|------|
| 1 | Installing Docker on Ubuntu with AppArmor | Step-by-step guide to install Docker on Ubuntu 24.04 and apply a custom AppArmor profile to containers. | [demo-1](demos/demo-1.md) |
| 2 | Example AppArmor Profiles | Example AppArmor profiles for common Docker workloads (database, web API, message broker). | [demo-2](demos/demo-2.md) |
3 | Building Safer images | A side by side comparison of a minimal but safer Dockerfile with a Standard one. | [demo-3](demos/demo-3/Readme.md)
| 4 | Resource Limits & Isolation | Demonstrates running a container with CPU, memory, process, and user isolation. | [demo-4](demos/demo-4.md) |
| 5 | Container Networking Security Best Practices | Shows how to secure container networking using user-defined networks, internal networks, network policies, and monitoring. | [demo-5](demos/demo-5.md) |
| 6 | Container Hardening | Demonstrates best practices for hardening containers: minimal images, non-root user, dropping capabilities, read-only filesystem, and image scanning. | [demo-6](demos/demo-6.md) |
| 7 | Falco Security Demo | Shows how to use Falco to detect suspicious activity in containers. | [demo-7](demos/demo-7.md) |

