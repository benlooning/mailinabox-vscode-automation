# Mailinabox VSCode Automation

This repository, named **mailinabox-vscode-automation**, contains a series of tasks to automate common administrative actions for managing remote servers using Visual Studio Code and Mail-in-a-Box. These tasks include initializing an SSH agent, updating and upgrading servers, managing Mail-in-a-Box installations, and syncing backups. The configuration is handled through a `tasks.json` file that is intended for use with Visual Studio Code.

## Prerequisites

To set up Visual Studio Code for use with this repository, you need to install and configure the following extensions:

- **Remote - SSH**: This extension allows you to connect to remote servers over SSH directly from VS Code.
  - Install it from the Extensions Marketplace (`ms-vscode-remote.remote-ssh`).
  - Set up your SSH configuration by creating or modifying `~/.ssh/config` with the connection details of your servers.
- **Task Runner**: VS Code's built-in task runner is used to execute the tasks defined in `tasks.json`. No additional setup is needed for this feature.

To use this repository effectively, you'll need:

- **Visual Studio Code**: The tasks are configured to work with VS Code's built-in task runner.
- **SSH Access**: Properly configured SSH access to your remote servers, including an SSH key.
- **Server Configuration File**: A `server_config.env` file that contains necessary environment variables for SSH connections.

## Repository Structure

- **`tasks.json`**: Contains definitions for automating various tasks, such as initializing the SSH agent, running server updates, managing Mail-in-a-Box, and synchronizing backups.
- **`server_config.env`**: Stores sensitive SSH connection details, such as the SSH key path and the SSH user.
- **`backups/`**: Local directory for storing synchronized Mail-in-a-Box backups from remote servers. Each server's backup is saved in a separate subdirectory.
- **`.gitignore`**: Ensures that sensitive files and backup directories are not added to version control.

## Tasks Overview

### 1. Init SSH Agent

**Label**: `Init SSH Agent`

- **Description**: Starts the SSH agent if it is not running and adds the SSH key if it is not already loaded. This task ensures that secure access to remote servers is available for all subsequent tasks.
- **How to Run**: This task runs automatically before any dependent task.

### 2. Update & Upgrade Remote Server

**Label**: `Update & Upgrade Remote Server`

- **Description**: Connects to the remote server and runs system updates and upgrades (`apt-get update && apt-get upgrade`).
- **Dependency**: This task depends on `Init SSH Agent`.
- **Inputs**: Prompts for the server IP address or hostname.

### 3. Update Mail-in-a-Box

**Label**: `Update Mail-in-a-Box`

- **Description**: Runs the Mail-in-a-Box setup script on the remote server to apply updates or configuration changes.
- **Dependency**: This task depends on `Init SSH Agent`.
- **Inputs**: Prompts for the server IP address or hostname.

### 4. Reboot Remote Server

**Label**: `Reboot Remote Server`

- **Description**: Reboots the remote server to apply changes.
- **Dependency**: This task depends on `Init SSH Agent`.
- **Inputs**: Prompts for the server IP address or hostname.

### 5. Sync Mail-in-a-Box Backup

**Label**: `Sync Mail-in-a-Box Backup`

- **Description**: Uses `rsync` to synchronize the Mail-in-a-Box backup directory from the remote server to a local directory (`backups/mailinabox/`). Creates a separate backup folder for each server using the server IP or hostname.
- **Dependency**: This task depends on `Init SSH Agent`.
- **Inputs**: Prompts for the server IP address or hostname.

## Setup Instructions for VS Code

1. **Install Required Extensions**:
   - **Remote - SSH**: Install the `Remote - SSH` extension (`ms-vscode-remote.remote-ssh`) from the Extensions Marketplace.
   - **Configure SSH**: Add your server connection details to the `~/.ssh/config` file. Example:
     ```
     Host my-server
         HostName 192.168.1.100
         User root
         IdentityFile ~/.ssh/id_rsa_box
     ```

2. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   ```
3. **Create the Server Configuration File** (`server_config.env`):
   ```env
   # Example content for server_config.env
   SSH_KEY_PATH=~/.ssh/id_rsa_box
   SSH_USER=root
   ```
4. **Set Up `.gitignore`**:
   Ensure that sensitive files are not versioned. The `.gitignore` already includes entries for:
   - `.env` files
   - SSH-related files (`.ssh/`)
   - Backup directories (`backups/`)

## Usage Instructions

1. **Clone the Repository**:
   ```bash
   git clone <repository-url>
   ```
2. **Create the Server Configuration File** (`server_config.env`):
   ```env
   # Example content for server_config.env
   SSH_KEY_PATH=~/.ssh/id_rsa_box
   SSH_USER=root
   ```
3. **Set Up `.gitignore`**:
   Ensure that sensitive files are not versioned. The `.gitignore` already includes entries for:
   - `.env` files
   - SSH-related files (`.ssh/`)
   - Backup directories (`backups/`)

4. **Run Tasks in VS Code**:
   - Open **Command Palette** (`Cmd + Shift + P` or `Ctrl + Shift + P`).
   - Type `Tasks: Run Task` and choose the desired task from the list.

## Important Notes

- **Security**: The `server_config.env` file contains sensitive information. Ensure it is not shared publicly and is listed in `.gitignore`.
- **Backups Directory**: The `backups/` directory is configured to store server-specific backups in separate folders under `backups/mailinabox/`. This directory is ignored by Git to prevent accidental versioning of potentially large and sensitive backup files.

## Troubleshooting

- **SSH Issues**: Ensure that the SSH key is correctly added to the agent (`ssh-add ~/.ssh/id_rsa_box`).
- **Permission Denied**: Make sure your user has the necessary permissions on the remote server to run the commands specified in each task.
- **Rsync Failures**: If `rsync` fails, verify that the remote server is reachable and the backup path `/home/user-data/backup` exists.

## Contributing

If you'd like to contribute to this project, feel free to open a pull request or issue on GitHub. Please ensure any contributions adhere to best practices for automation and security.

## License

This project is licensed under the MIT License. See the `LICENSE` file for more details.

