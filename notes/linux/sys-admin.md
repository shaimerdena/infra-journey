# System Administration

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [System Administration](#system-administration)
  - [Understanding System Administration](#understanding-system-administration)
    - [Gaining admin access](#gaining-admin-access)
    - [Responsibilities of a Linux System Administrator](#responsibilities-of-a-linux-system-administrator)
  - [Using the Root Account](#using-the-root-account)
    - [`su` command](#su-command)

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

After logging in as the **root** user:
- **Home directory:** `/root`
- **Default shell:** `/bin/bash`

The root user's information is stored in the **`/etc/passwd`** file.

Example:

```text
root:x:0:0:root:/root:/bin/bash
```

| Field | Value | Description |
|--------|-------|-------------|
| Username | `root` | User name |
| Password | `x` | Password is stored in `/etc/shadow` (encrypted) |
| UID | `0` | User ID (root) |
| GID | `0` | Primary group ID (root group) |
| Comment | `root` | User description |
| Home directory | `/root` | Root user's home directory |
| Login shell | `/bin/bash` | Default shell |

<i> Notes:
- The **encrypted password** is stored in **`/etc/shadow`**, not in `/etc/passwd`.
- Although you **can** edit `/etc/passwd` manually, it is **recommended** to use commands like **`usermod`** to change a user's home directory or login shell.
- type `exit` to log out of the root account and return to your regular user account.
</i>

### `su` command

The **`su` (switch user)** command allows you to switch to another user (by default, `root`) without logging out.

```bash
su [options] [username]
```

| Command | Description |
|---------|-------------|
| `su` | Switch to the root user **without** loading the root user's environment. |
| `su -` | Switch to the root user **and load** the root user's login environment (recommended). |
| `su - username` | Switch to another user's account and load their environment. |

---

`su` vs `su -`

| `su` | `su -` |
|------|---------|
| Changes user only | Changes user **and** loads their environment |
| Keeps current working directory | Changes to the user's home directory |
| Keeps current environment variables | Loads the new user's environment variables (e.g., `PATH`) |
| May cause `command not found` errors | Behaves like a normal login |

<i> Notes:

- `su` requires the **target user's password**.
- If you're already **root**, you can switch to another user without entering a password.
- Exit the root shell with:
  ```bash
  exit
  ```
  or press **Ctrl + D**.

- Never leave an open **root shell** unattended on a shared computer.
</i>