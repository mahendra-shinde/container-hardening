# Module 3: Secure Docker Images

## Building Secure Docker Images: Best Practices
- Use official, minimal base images (e.g., `alpine`, `distroless`).
- Avoid installing unnecessary packages.
- Regularly update images and rebuild containers.
- Use multi-stage builds to reduce final image size.

## Vulnerability Scanning of Docker Images
- Trivy: `trivy image <image-name>`
- Snyk: `snyk test --docker <image-name>`
- Clair: Integrate with CI/CD for automated scanning.

## Reducing Image Size and Using Minimal Base Images
- Remove build tools and caches after installation.
- Use `.dockerignore` to exclude unnecessary files.

## Managing Secrets in Images and Containers
- Never hardcode secrets in images.
- Use Docker secrets, environment variables, or secret management tools (e.g., HashiCorp Vault).
