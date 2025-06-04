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

### Additional Docker Daemon Hardening Techniques

- Bind the Docker daemon to a specific IP address instead of all interfaces.
- Enforce TLS for all Docker API access by configuring certificates (`--tlsverify`, `--tlscacert`, `--tlscert`, `--tlskey`).
- Disable legacy or unused features (e.g., experimental features, legacy registry support).
- Use user namespaces to isolate container and host users.
- Audit Docker daemon logs regularly for suspicious activity.
- Limit the number of concurrent containers if possible.
- Use firewall rules to restrict access to Docker daemon ports.

## Steps to Disable the Default Unix Socket or Restrict Its Permissions on Ubuntu 24.04

### 1. Restrict Permissions on the Docker Unix Socket

By default, the Docker daemon listens on `/var/run/docker.sock`. To restrict access:

```bash
sudo chmod 660 /var/run/docker.sock
sudo chown root:docker /var/run/docker.sock
```
Only users in the `docker` group will be able to access the socket.

### 2. Disable the Default Unix Socket

To disable the default Unix socket and use only a TCP socket:

1. Edit the Docker service file override:
    ```bash
    sudo systemctl edit docker
    ```
2. Add the following lines:
    ```
    [Service]
    ExecStart=
    ExecStart=/usr/bin/dockerd -H tcp://127.0.0.1:2376
    ```
    This disables the Unix socket and binds Docker to a secure TCP socket.

3. Reload systemd and restart Docker:
    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    ```

**Note:** Always secure the TCP socket with TLS to prevent unauthorized access.