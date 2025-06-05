## Setting Up Docker Desktop with Windows Containers
This guide walks you through enabling Windows features, installing Docker Desktop, and configuring it for Windows containers.

---

### 1. Enable Required Windows Features

Open **PowerShell as Administrator** and run:

```powershell
Enable-WindowsOptionalFeature -FeatureName Containers -Online
Enable-WindowsOptionalFeature -FeatureName Microsoft-Hyper-V -Online
```

- `Containers`: Enables Windows container support.
- `Microsoft-Hyper-V`: Required for virtualization.

---

### 2. Download Docker Desktop

- Visit the [Docker Desktop for Windows (AMD64)](https://www.docker.com/products/docker-desktop/) download page.
- Download the installer appropriate for your system.

---

### 3. Install Docker Desktop

- Run the downloaded installer.
- During installation, select:
    1. **Use WSL 2 (Linux Containers)** *(recommended for Linux containers)*
    2. **Use Windows Containers** *(required for Windows containers)*
    3. **Add Desktop Icon**

> **Note:** You can switch between Linux and Windows containers after installation.

- Click **Start Installation**.
- The system may restart during installation.

---

### 4. Initial Docker Desktop Setup

- After restart, launch Docker Desktop from the desktop icon.
- Wait for Docker to finish initializing (icon animation stops in the system tray).
- **Switch to Windows Containers**:
    - Right-click the Docker Desktop icon in the system tray.
    - Select **"Switch to Windows Containers..."**.

---

### 5. Verify Docker Installation

Open **Command Prompt** and run:

```shell
docker version
```

- Confirms Docker is installed and running.

---

### 6. Pull a Windows Container Image

To pull the Windows Server Core image, run:

```shell
docker pull mcr.microsoft.com/windows/servercore:ltsc2022
```

### 7. Run Windows Containers with Different Isolation Modes

Windows containers can run in two isolation modes: **Process (Native) Isolation** and **Hyper-V Isolation**. The main difference is how processes inside the container are exposed to the host operating system.

#### **Process (Native) Isolation**

In this mode, container processes run directly on the host OS. This means processes inside the container are visible and manageable from the host.

**Steps:**

1. **Start a container named `c1`:**

    ```shell
    docker run -it --name c1 mcr.microsoft.com/windows/servercore:ltsc2022 cmd
    ping google.com -t
    ```

2. **Open Task Manager on the host OS.**
3. **Search for the `PING` process.**  
   You should see the `PING` process running, as it is visible to the host.
4. **End the `PING` process from Task Manager.**  
   The container will report that the `PING` process has stopped.
5. **Exit the container by typing `exit`.**

#### **Hyper-V Isolation**

In this mode, each container runs inside a lightweight virtual machine. Processes inside the container are isolated from the host and are not visible in Task Manager.

**Steps:**

1. **Start a container named `c2` with Hyper-V isolation:**

    ```shell
    docker run -it --name c2 --isolation hyperv mcr.microsoft.com/windows/servercore:ltsc2022 cmd
    ping google.com -t
    ```

2. **Open Task Manager on the host OS.**
3. **Search for the `PING` process.**  
   You will not find the `PING` process, as it is isolated within the container's VM.
4. **Exit the container by typing `exit`.**

---

**Summary:**  
- **Process Isolation:** Container processes are visible and manageable from the host.
- **Hyper-V Isolation:** Container processes are hidden from the host, providing stronger isolation.
