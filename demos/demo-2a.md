## Demo: Creating a Custom Nginx Container with a Non-Root User

### 1. Start a Container from the `nginx:alpine` Image

```sh
docker run --name c1 -it nginx:alpine sh
```

Inside the container:

```sh
$ whoami
root

$ adduser mahendra
Enter password: Password1
Re-enter password: Password1

$ exit
```

---

### 2. Commit the Container as a New Image

```sh
docker commit c1 test1
```

---

### 3. Run a New Container as the `mahendra` User

```sh
docker run -it -u mahendra test1 sh
```

Inside the container:

```sh
$ whoami
mahendra

$ apk add curl
ERROR: Permission denied
```

> **Note:** The `mahendra` user does not have permission to install packages.