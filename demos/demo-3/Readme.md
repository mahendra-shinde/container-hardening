## Comparison of Dockerfiles: Minimal vs Standard

### Why the Minimal Dockerfile is More Secure

1. **Base Image**
   - **Standard:** Uses `ubuntu:22.04`, a general-purpose OS with a larger attack surface.
   - **Minimal:** Uses `eclipse-temurin:17-jdk-alpine`, a slim, purpose-built image with fewer packages, reducing vulnerabilities.

2. **User Privileges**
   - **Standard:** Runs Tomcat as root (default), which is risky if the container is compromised.
   - **Minimal:** Creates and uses a non-root `tomcat` user, following the principle of least privilege.

3. **Package Management**
   - **Standard:** Installs `openjdk-11-jdk` and `curl`, then cleans apt cache, but still has more packages than needed.
   - **Minimal:** Installs only `curl` (with `apk`), and uses a JDK base image, minimizing installed software.

4. **Tomcat Download & Verification**
   - **Standard:** Downloads Tomcat without verifying integrity.
   - **Minimal:** Downloads Tomcat and verifies it with a SHA-512 checksum, protecting against tampered downloads.

5. **File Permissions**
   - **Standard:** No explicit permission hardening.
   - **Minimal:** Sets ownership and executable permissions for Tomcat files, reducing risk of privilege escalation.

6. **Health Check**
   - **Standard:** No health check.
   - **Minimal:** Adds a `HEALTHCHECK` to ensure Tomcat is running, improving operational security.

7. **Labels**
   - **Minimal:** Adds maintainer and version labels for traceability (not a direct security feature, but good practice).

**Summary:**
The minimal Dockerfile is more secure because it uses a smaller, purpose-built base image, runs as a non-root user, verifies downloads, hardens file permissions, and includes a health check. These practices reduce the attack surface and follow container security best practices.

# Tomcat 11 on JDK 17 Alpine - Dockerfile

## Description

This repository contains the Dockerfile and related files to build a minimal Docker image for running Apache Tomcat 11 on JDK 17, based on Alpine Linux. Alpine is a security-oriented, lightweight Linux distribution, which makes it an excellent base for building secure and efficient Docker images.

## Minimal Dockerfile Advantages

- **Smaller Attack Surface:** Fewer packages and a smaller base image reduce potential vulnerabilities.
- **Non-root User:** Running Tomcat as a non-root user minimizes the impact of a potential container breach.
- **Verified Downloads:** Ensures that the Tomcat binary has not been tampered with.
- **Optimized Layers:** Fewer layers and smaller image size lead to faster download and deployment times.
- **Health Check:** Built-in check to verify that Tomcat is running correctly inside the container.

## Getting Started

To build and run the Docker image, use the following commands:

```bash
# Build the Docker image
docker build -t my-tomcat .

# Run the Docker container
docker run -d -p 8080:8080 my-tomcat
```

## Customization

You can customize the image by modifying the `Dockerfile` or by providing your own `context.xml`, `server.xml`, or web applications to be deployed in the `webapps` directory.
