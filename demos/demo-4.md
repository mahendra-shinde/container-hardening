# Docker Container Demo: Resource Limits & Isolation

This demo shows how to run a Docker container with CPU, memory limits, and process/user isolation.

## 1. Create a Sample Dockerfile

```dockerfile
# Dockerfile
FROM alpine:latest
CMD ["sh", "-c", "while true; do echo Hello from container; sleep 5; done"]
```

## 2. Build the Image

```sh
docker build -t demo-resource-limits .
```

## 3. Run the Container with Resource Limits

```sh
docker run -d \
    --name demo-limited \
    --cpus="0.5" \
    --memory="100m" \
    --pids-limit=50 \
    --read-only \
    --user 1001:1001 \
    demo-resource-limits
```

**Options explained:**
- `--cpus="0.5"`: Limit to half a CPU core.
- `--memory="100m"`: Limit memory to 100MB.
- `--pids-limit=50`: Limit to 50 processes.
- `--read-only`: Filesystem is read-only.
- `--user 1001:1001`: Run as non-root user.

## 4. Inspect the Running Container

```sh
docker stats demo-limited
docker exec -it demo-limited sh
```

## 5. Clean Up

```sh
docker rm -f demo-limited
```