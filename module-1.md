# Module 1: Introduction to Container Security

## Overview of Containerization and Docker
- Containerization packages applications and dependencies into isolated units called containers.
- Docker is the most popular container platform, enabling consistent environments from development to production.

## Understanding the Docker Architecture and Security Model
- Docker Engine: Core component that runs and manages containers.
- Images: Read-only templates for containers.
- Containers: Running instances of images.
- Security Model: Relies on Linux kernel features (namespaces, cgroups) for isolation, but shares the host kernel.

### Additional Details on Docker Security Model
- **Isolation Mechanisms:** Uses namespaces (PID, network, mount, user, etc.) to isolate containers from each other and the host.
- **Resource Control:** Employs cgroups to limit CPU, memory, and disk usage per container.
- **Capabilities:** Drops unnecessary Linux capabilities by default to reduce attack surface.
- **User Namespaces:** Can map container users to non-root users on the host for enhanced security.
- **Seccomp Profiles:** Restricts system calls available to containers, minimizing potential exploits.
- **AppArmor/SELinux:** Supports integration with Linux security modules for mandatory access controls.

## Threat Landscape for Containers
- Image vulnerabilities (outdated packages, malware).
- Insecure defaults (root user, open ports).
- Host compromise via container escape.
- Network attacks (lateral movement, data exfiltration).
