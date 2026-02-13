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
        image: mahendrshinde/myweb:latest
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
        image: mahendrshinde/myweb:latest
        read_only: true
```

---

### 5. Scan Images for Vulnerabilities

Download trivy for windows from [Windows Version](https://github.com/aquasecurity/trivy/releases/download/v0.69.1/trivy_0.69.1_windows-64bit.zip)
After download, extract the contents to `C:\tools` directory.
Make sure `c:\tools` directory is added to your windows ENV variable `path`

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
