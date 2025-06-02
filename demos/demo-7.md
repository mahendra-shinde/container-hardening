# Falco Security Demo

This demo shows how to use [Falco](https://falco.org/) to detect suspicious activity in a containerized environment.

## Prerequisites

- Docker installed
- Falco installed ([installation guide](https://falco.org/docs/getting-started/installation/))

## 1. Start Falco

```sh
sudo falco
```

Falco will start monitoring your system for suspicious activity.

## 2. Run a Test Container

Open a new terminal and run:

```sh
docker run -it --rm alpine sh
```

## 3. Trigger a Falco Alert

Inside the container, try to read `/etc/shadow`:

```sh
cat /etc/shadow
```

## 4. Observe Falco Output

In the Falco terminal, you should see an alert similar to:

```
13:37:42.123456789: Warning File below /etc opened for reading (user=root command=cat /etc/shadow ...)
```

## 5. Clean Up

- Exit the container: `exit`
- Stop Falco: `Ctrl+C`

## Resources

- [Falco Rules](https://falco.org/docs/rules/)
- [Falco Documentation](https://falco.org/docs/)
