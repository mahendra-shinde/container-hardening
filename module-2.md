# Module 2: Securing the Docker Host

## Best Practices for Securing the Host OS
- Use a minimal, hardened OS (e.g., Ubuntu Minimal, Alpine, Container-Optimized OS).
- Regularly update and patch the host.
- Limit installed packages to reduce attack surface.
- Disable unused services and ports.

## Docker Daemon Hardening Techniques
- Run Docker with the least privileges.
- Restrict access to the Docker socket (`/var/run/docker.sock`).
- Use TLS to secure Docker API endpoints.
- Enable Docker Content Trust (`DOCKER_CONTENT_TRUST=1`).

## Implementing Kernel-Level Security with AppArmor and SELinux
- AppArmor: Apply Docker’s default or custom AppArmor profiles to restrict container capabilities.
- SELinux: Use SELinux in enforcing mode with Docker (`--selinux-enabled`).
