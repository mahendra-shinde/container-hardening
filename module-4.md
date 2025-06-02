# Module 4: Secure Container Runtime

## Applying Resource Limits with cgroups
- Use `--memory`, `--cpus`, and `--pids-limit` flags to restrict container resources.

## Enabling User Namespaces for Container Isolation
- Enable user namespaces in Docker (`--userns-remap=default`).

## Running Containers as Non-Root Users
- Specify a non-root user in Dockerfile: `USER appuser`
- Avoid running processes as root inside containers.
