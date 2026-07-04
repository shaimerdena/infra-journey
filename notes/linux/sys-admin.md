# System Administration

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [System Administration](#system-administration)
  - [Understanding System Administration](#understanding-system-administration)
    - [Gaining admin access](#gaining-admin-access)
    - [Responsibilities of a Linux System Administrator](#responsibilities-of-a-linux-system-administrator)
  - [Using the Root Account](#using-the-root-account)

## Understanding System Administration

System administration is the process of managing and maintaining computer systems, including hardware, software, and networks. It involves a variety of tasks, such as installing and configuring software, monitoring system performance, and troubleshooting issues.

Key responsibilities of a system administrator include:

- User management: Creating and managing user accounts, permissions, and access controls.
- System monitoring: Keeping an eye on system performance, resource usage, and security logs.
- Backup and recovery: Implementing backup solutions and ensuring data can be restored in case of failure.
- Security: Applying security patches, configuring firewalls, and monitoring for unauthorized access.
- Documentation: Maintaining clear and up-to-date documentation for systems and procedures.

### Gaining admin access

Linux administrators usually **log in as a regular user** and only obtain administrator (root) privileges when necessary.

| Method | Description | Best Use |
|--------|-------------|----------|
| `su` | Switches to another user (usually `root`) and opens a **root shell**. You can execute multiple administrative commands until you exit the shell. | Long administrative sessions. |
| `sudo` | Executes **one command** with root privileges, then returns to the regular user. | Safer for everyday administration. |
| **Cockpit** | A **web-based administration interface** for managing Linux systems (storage, networking, users, services, monitoring, etc.). | Remote system administration through a browser. |
| **Graphical Tools** | Older GUI administration tools (`system-config-*`). Mostly deprecated in modern Fedora/RHEL, but still available in some older versions. | Legacy systems. |

### Responsibilities of a Linux System Administrator

A system administrator is responsible for configuring, maintaining, and securing the Linux system.

| Area | Responsibilities |
|------|------------------|
| **Filesystems** | Manage storage, partitions, and filesystem layout. Create backups. Access and manage any user's files when necessary. |
| **Software Installation** | Install, update, and remove software available to all users. Prevent unauthorized or malicious software installation. |
| **User Accounts** | Create, modify, and delete user and group accounts. Manage user permissions. |
| **Network Interfaces** | Configure network connections and interfaces. Manage wired and wireless networking. |
| **Servers** | Install, configure, start, stop, and maintain services such as web, mail, DNS, and file servers. |
| **Security** | Configure firewalls, access control, monitor system activity, and protect system resources from misuse or attacks. |

<i> Notes: 
- Regular users can install software only in their own directories (if permitted).
- Server processes often run under dedicated service accounts (e.g., `apache`, `rpc`) instead of `root` for better security.
- The root user has full access to all files and system resources.
</i>

## Using the Root Account

The root account is the superuser in Linux, with unrestricted access to all commands and files. It is used for system administration tasks that require elevated privileges.