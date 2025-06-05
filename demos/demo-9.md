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

- **Native (Process) Isolation:** All child processes of the container are visible from the host OS.

    ```shell
    docker run -it --name c1 mcr.microsoft.com/windows/servercore:ltsc2022 cmd
    ```

- **Hyper-V Isolation:** Child processes inside the container are not visible from the host OS.

    ```shell
    docker run -it --name c2 --isolation hyperv mcr.microsoft.com/windows/servercore:ltsc2022 cmd
    ```