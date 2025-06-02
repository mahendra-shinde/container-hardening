# Module 5: Docker Networking and Storage Security

## Configuring Secure Container Networking
- Use user-defined bridge networks for isolation.
- Limit container communication with network policies.

## Isolating Containers with Network Namespaces
- Each container gets its own network namespace by default.
- Use `--network` flag to control network access.

## Encrypting Data at Rest and in Transit
- Use encrypted storage drivers or file systems.
- Enable TLS for all network communication (Docker API, application traffic).
