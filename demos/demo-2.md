# Example AppArmor Profiles for Docker Containers

Here are few example AppArmor profiles tailored for common Docker container workloads. Each profile demonstrates how to restrict container privileges by specifying allowed capabilities, file access, and network permissions. Use these as starting points to enhance the security posture of your containerized applications.

## 1. Database (e.g., MySQL)

```
profile mysql-docker flags=(attach_disconnected,mediate_deleted) {
  #include <tunables/global>

  network inet,
  capability net_bind_service,
  capability setgid,
  capability setuid,
  /usr/sbin/mysqld ix,
  /var/lib/mysql/ r,
  /var/lib/mysql/** rwk,
  /etc/mysql/** r,
  /tmp/ rw,
  deny /bin/dash x,
}
```

## 2. Web API (e.g., Node.js Express)

```
profile node-webapi-docker flags=(attach_disconnected,mediate_deleted) {
  #include <tunables/global>

  network inet,
  capability net_bind_service,
  /usr/bin/node ix,
  /app/** r,
  /tmp/ rw,
  deny /bin/bash x,
}
```

## 3. Message Broker (e.g., RabbitMQ)

```
profile rabbitmq-docker flags=(attach_disconnected,mediate_deleted) {
  #include <tunables/global>

  network inet,
  capability net_bind_service,
  /usr/lib/rabbitmq/bin/rabbitmq-server ix,
  /var/lib/rabbitmq/ r,
  /var/lib/rabbitmq/** rwk,
  /etc/rabbitmq/** r,
  /tmp/ rw,
  deny /bin/sh x,
}
```

> **Note:** These are minimal examples. Adjust paths, permissions, and capabilities as needed for your application and environment.
