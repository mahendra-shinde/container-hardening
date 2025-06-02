# Demo: Installing Docker on Ubuntu Linux 24.04 with AppArmor Profile

This demo provides step-by-step instructions for installing Docker on Ubuntu 24.04 and configuring it to use a custom AppArmor security profile. It covers updating the system, installing Docker, verifying the installation, and applying AppArmor profiles to containers for enhanced security. The guide also demonstrates how to create, load, and test a custom AppArmor profile to restrict container capabilities.

## 1. Update System Packages
```sh
sudo apt update && sudo apt upgrade -y
```

## 2. Install Required Packages
```sh
sudo apt install -y ca-certificates curl gnupg lsb-release apparmor apparmor-utils
```

## 3. Add Docker’s Official GPG Key
```sh
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

## 4. Set Up the Docker Repository
```sh
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

## 5. Install Docker Engine
```sh
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 6. Verify Docker Installation
```sh
sudo docker run hello-world
```

## 7. Enable and Start Docker
```sh
sudo systemctl enable docker
sudo systemctl start docker
```

---

## Check the AppArmor Status and List profiles

1. Check AppArmor Status

    ```sh
    sudo aa-status
    ```

2. List Available AppArmor Profiles

    ```sh
    sudo aa-status | grep docker
    ```

## Run a Container with a Custom AppArmor Profile

1. Create a simple AppArmor profile (e.g., `my-docker-profile`):

    ```sh
    sudo nano /etc/apparmor.d/my-docker-profile
    ```

2. Paste a basic profile (example):

    ```
    #include <tunables/global>
    profile my-docker-profile flags=(attach_disconnected,mediate_deleted) {
    #include <abstractions/base>
    capability,
    network,
    file,
    umount,
    deny /bin/su rix,
    }
    ```

3. Load the profile:

    ```sh
    sudo apparmor_parser -r /etc/apparmor.d/my-docker-profile
    ```

3. Run Docker with the profile:

    ```sh
    sudo docker run --rm --security-opt apparmor=my-docker-profile ubuntu:24.04 uname -a
    ```

## Test a Disabled Feature (e.g., `su` command)

Try running a command inside the container that is denied by the profile (such as `/bin/su`):

    ```sh
    sudo docker run --rm --security-opt apparmor=my-docker-profile ubuntu:24.04 su
    ```

You should see a permission denied error, confirming the AppArmor profile is active.


## References
- [Docker Docs: Install on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
- [Docker Docs: AppArmor security profiles](https://docs.docker.com/engine/security/apparmor/)
