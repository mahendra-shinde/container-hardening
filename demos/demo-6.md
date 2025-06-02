# Demo: Container Hardening

## Objective

Demonstrate best practices for hardening containers to improve security.

---

## Steps

### 1. Use Minimal Base Images

```dockerfile
# Use a minimal base image
FROM alpine:3.19
RUN apk add --no-cache curl
```

---

### 2. Set Non-Root User

```dockerfile
# Add and use a non-root user
RUN adduser -D appuser
USER appuser
```

---

### 3. Limit Container Capabilities

```yaml
# docker-compose.yml example
services:
    app:
        image: myapp:latest
        cap_drop:
            - ALL
        cap_add:
            - NET_BIND_SERVICE
```

---

### 4. Read-Only Filesystem

```yaml
services:
    app:
        image: myapp:latest
        read_only: true
```

---

### 5. Scan Images for Vulnerabilities

```sh
# Using Trivy to scan for vulnerabilities
trivy image myapp:latest
```

---

## Summary

- Use minimal images
- Run as non-root
- Drop unnecessary capabilities
- Use read-only filesystems
- Scan images regularly

---

**Try these steps to harden your containers!**