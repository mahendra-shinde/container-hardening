# Module 4: Secure Container Runtime

## Applying Resource Limits with cgroups
- Use `--memory`, `--cpus`, and `--pids-limit` flags to restrict container resources.

## Enabling User Namespaces for Container Isolation
- Enable user namespaces in Docker (`--userns-remap=default`).

## Running Containers as Non-Root Users
- Specify a non-root user in Dockerfile: `USER appuser`
- Avoid running processes as root inside containers.

## Steps to Enable User Namespaces for Container Isolation

1. **Edit the Docker daemon configuration:**
    - Open or create `/etc/docker/daemon.json` and add:
      ```json
      {
         "userns-remap": "default"
      }
      ```
2. **Restart the Docker service:**
    - On Linux:  
      ```sh
      sudo systemctl restart docker
      ```
3. **Verify user namespace support:**
    - Run a container and check the mapped user IDs:
      ```sh
      docker run --rm alpine id
      ```
    - The output should show a non-root UID (not 0).

4. **Review Docker documentation for advanced mapping options as needed.**