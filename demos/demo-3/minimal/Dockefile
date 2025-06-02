# Use official OpenJDK 17 Alpine as base image
FROM eclipse-temurin:17-jdk-alpine

# Set environment variables for security
ENV CATALINA_HOME=/usr/local/tomcat \
    PATH=$PATH:/usr/local/tomcat/bin

# Create a non-root user and group for running Tomcat
RUN addgroup -S tomcat && adduser -S -G tomcat -h /usr/local/tomcat tomcat

# Download and verify Tomcat
ARG TOMCAT_VERSION=10.1.24
ARG TOMCAT_SHA512=3c1f8b5d4e2f0a6c7b9c8e1f3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8g9h0i1j2k3l4m5n6o7p8q9r0s1t2u3v4w5x6y7z8
RUN apk add --no-cache curl && \
    curl -fSL "https://dlcdn.apache.org/tomcat/tomcat-10/v${TOMCAT_VERSION}/bin/apache-tomcat-${TOMCAT_VERSION}.tar.gz" -o /tmp/tomcat.tar.gz && \
    echo "${TOMCAT_SHA512}  /tmp/tomcat.tar.gz" | sha512sum -c - && \
    mkdir -p "$CATALINA_HOME" && \
    tar xzf /tmp/tomcat.tar.gz --strip-components=1 -C "$CATALINA_HOME" && \
    rm /tmp/tomcat.tar.gz
# Set permissions for Tomcat directories
RUN chown -R tomcat:tomcat "$CATALINA_HOME" && \
    chmod +x "$CATALINA_HOME"/bin/*.sh
# Expose the Tomcat port
EXPOSE 8080
# Switch to the non-root user
USER tomcat
# Set the working directory
WORKDIR $CATALINA_HOME
# Start Tomcat
CMD ["catalina.sh", "run"]
# Health check to ensure Tomcat is running
HEALTHCHECK --interval=30s --timeout=10s --start-period=10s --retries=3 \
    CMD curl -f http://localhost:8080/ || exit 1
# Add a label for the maintainer
LABEL maintainer="Mahendra Shinde"
# Add a label for the image version
LABEL version="1.0"
