# System Administration

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [System Administration](#system-administration)
  - [Understanding System Administration](#understanding-system-administration)
    - [Gaining admin access](#gaining-admin-access)
    - [Responsibilities of a Linux System Administrator](#responsibilities-of-a-linux-system-administrator)
  - [Using the Root Account](#using-the-root-account)
    - [`su` command](#su-command)
    - [`sudo` command](#sudo-command)
      - [Common Rules](#common-rules)
      - [Using `sudo`](#using-sudo)
    - [`su` vs `sudo`](#su-vs-sudo)
  - [Admin commands, config files and logs](#admin-commands-config-files-and-logs)
    - [Admin commands](#admin-commands)
      - [Common Administrative Commands](#common-administrative-commands)
      - [Installing Your Own Commands](#installing-your-own-commands)
    - [Admin Configuration Files](#admin-configuration-files)
    - [Admin Log files and systemd journal](#admin-log-files-and-systemd-journal)
      - [`journalctl` command](#journalctl-command)
      - [`rsyslog`](#rsyslog)
  - [Using other admin accounts](#using-other-admin-accounts)
  - [Checking and Configuring Hardware](#checking-and-configuring-hardware)
    - [Checking the Hardware](#checking-the-hardware)

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

### `sudo` command

`sudo` allows a regular user to execute commands with **root privileges** without logging in as the root user.

What `sudo` can do?

- Run **any command** as root.
- Allow only **specific commands** to be executed as root.
- Use the **user's own password** instead of the root password.
- Optionally allow commands **without asking for a password** (`NOPASSWD`).
- Log **which user** executed administrative commands.

<i> Note: Unlike `su`, `sudo` records exactly **who** ran each command. </i>

---

<strong> Configuration </strong>

Administrative permissions are configured in: `/etc/sudoers`

Always edit this file using:

```bash
visudo
```

<strong> Why `visudo`? </strong>

- Locks the file to prevent simultaneous edits.
- Checks the syntax before saving.
- Prevents breaking the `sudoers` configuration.

---

#### Common Rules

Give a user full sudo privileges - `joe ALL=(ALL) ALL`

Meaning:

- **joe** → user
- **ALL** → any host
- **(ALL)** → can act as any user
- **ALL** → may run any command

---

Allow sudo without a password - `joe ALL=(ALL) NOPASSWD: ALL`

---

#### Using `sudo`

```bash
sudo apt update
sudo systemctl restart nginx
sudo useradd alice
```

The user enters **their own password**, not the root password.

---

<strong> Password Timeout </strong>

After entering the password once, `sudo` remembers it for a short period.

- **Fedora/RHEL:** 5 minutes (default)
- **Ubuntu:** configurable in `/etc/sudoers`

---

### `su` vs `sudo`

| `su` | `sudo` |
|------|---------|
| Switches to another user (usually `root`) | Runs **one command** as root |
| Requires the **root password** | Requires the **current user's password** |
| Opens a root shell | Returns to the normal user after the command finishes |
| Does not record which user executed commands | Logs which user executed each command |
| Better for long administrative sessions | Better for everyday administration |


## Admin commands, config files and logs 

### Admin commands

Administrative commands are mainly used by the **root user** or users with **`sudo` privileges**.

| Directory | Purpose |
|-----------|---------|
| `/usr/sbin` | Contains most **system administration commands** (user management, services, networking, system configuration, etc.). |
| `/sbin` | Usually a symbolic link to `/usr/sbin` on modern Linux distributions. |

<i> Notes: In modern Ubuntu, Fedora, and RHEL, most administrative commands are located in **`/usr/sbin`**. </i>

---

#### Common Administrative Commands

| Command | Purpose |
|---------|---------|
| `useradd` | Create a new user |
| `usermod` | Modify a user |
| `userdel` | Delete a user |
| `passwd` | Change a user's password |
| `mount` | Mount a filesystem |
| `umount` | Unmount a filesystem |
| `systemctl` | Manage system services |
| `shutdown` | Shut down or reboot the system |

---

<strong> Manual Pages </strong>

Administrative commands are usually documented in **Section 8** of the Linux manual.

```bash
man 8 command
```

Example:

```bash
man 8 useradd
```

---

#### Installing Your Own Commands

If you create your own administrative scripts or install third-party tools, common locations are:

| Directory | Purpose |
|-----------|---------|
| `/usr/local/bin` | User-installed executable programs |
| `/usr/local/sbin` | User-installed administrative commands |
| `/opt` | Optional third-party applications |

These directories are usually included in the system's `PATH`.

### Admin Configuration Files

Main configuration locations:

| Directory | Purpose |
|-----------|---------|
| `~/` (Home directory) | User-specific configuration files. |
| `/etc` | System-wide configuration files used by the operating system and services. |

**Cheat sheet for common configuration files: [Linux Configuration Files Cheat Sheet](linux-conf-files-cheat-sheet.md)**

### Admin Log files and systemd journal

Log files record events that occur in the operating system.

They are useful for:

- Troubleshooting errors
- Debugging applications and services
- Monitoring system activity
- Detecting security issues

---

**Logging Systems**

| Tool | Purpose | Status |
|------|---------|--------|
| `journalctl` | View logs collected by **systemd journal**. The boot process, the kernel, and all systemd-managed services direct their status and error messages to the systemd journal. | 🟢 Modern (recommended) |
| `rsyslogd` | Traditional Linux logging daemon | 🟡 Still widely used |
| `syslogd` | Older logging daemon | 🔴 Legacy |

<i> Notes: Modern Linux distributions (Ubuntu, Fedora, RHEL) primarily use **systemd journal**. </i>

---

#### `journalctl` command

The `journalctl` command is used to view logs collected by **systemd journal**.

| Command | Description |
|---------|-------------|
| `journalctl` | Show all logs. |
| `journalctl -n 20` | Show the last 20 log entries. |
| `journalctl -f` | Follow new log messages in real time (like `tail -f`). |
| `journalctl -b` | Show logs from the current boot. |
| `journalctl --list-boots` | List all recorded system boots. |
| `journalctl -b <BOOT_ID>` | Show logs from a specific boot. |
| `journalctl -k` | Show only kernel messages. |
| `journalctl -u ssh.service` | Show logs for a specific service. |
| `journalctl PRIORITY=0` | Show only messages with a specific priority (0–7). |
| `journalctl -a` | Display all available fields (don't truncate output). |
| `journalctl -a -f` | Follow logs in real time and display all fields. |

---

Log Priorities

| Priority | Meaning |
|:-------:|---------|
| `0` | Emergency |
| `1` | Alert |
| `2` | Critical |
| `3` | Error |
| `4` | Warning |
| `5` | Notice |
| `6` | Informational |
| `7` | Debug |

---

Most Useful Commands (Daily Use)

```bash
journalctl              # All logs
journalctl -n 20        # Last 20 logs
journalctl -f           # Follow logs in real time
journalctl -b           # Current boot
journalctl -k           # Kernel logs
journalctl -u nginx     # Logs for nginx service
journalctl -u ssh       # Logs for SSH service
journalctl -p err       # Only error messages
```

<i> Tip: When troubleshooting a service, a common workflow is:

```bash
systemctl status nginx
journalctl -u nginx -n 30
```

First check the service status, then inspect its most recent logs. 
</i>

#### `rsyslog`

`rsyslog` is a logging daemon that collects system and application log messages.

It can:

- Store logs in files.
- Forward logs to remote log servers.
- Filter and organize log messages.

<i> Note: Modern Linux systems often use **`systemd-journald`** together with **`rsyslog`**. </i>

---

Configuration

| File | Purpose |
|------|---------|
| `/etc/rsyslog.conf` | Main `rsyslog` configuration file. |

---

Common Log Files

| File | Purpose |
|------|---------|
| `/var/log/boot.log` | Boot process messages. |
| `/var/log/messages` | General system messages. |
| `/var/log/secure` | Authentication and security-related events. |

---

<i> Notes:

- `journalctl` reads logs from **systemd journal**.
- `rsyslog` stores logs in **text files** (usually in `/var/log`).
- Many enterprise Linux systems use **both** `journald` and `rsyslog`.
</i>

## Using other admin accounts

Linux has special **system accounts** used by services and daemon processes.

These accounts are mainly created to:

- Own files and directories related to a service.
- Run service processes.
- Limit the permissions available to each service.
- Improve system security.

---

**Security Principle**

Services usually run as separate users instead of `root`.

For example:

```text
Apache → www-data
Postfix → postfix
Chrony → chrony
```

If one service is compromised, the attacker receives only the limited permissions of that service account rather than full root access. This follows the **Principle of Least Privilege**: each service should receive only the permissions it needs.

---

**Common Examples**

| System User | Service / Purpose |
|---|---|
| `root` | Superuser with full system access |
| `www-data` | Runs Apache web server processes on Ubuntu |
| `apache` | Runs Apache (`httpd`) processes on RHEL/Fedora |
| `postfix` | Runs the Postfix mail service |
| `chrony` | Runs the `chronyd` time synchronization service |
| `lp` | Owns files related to printing services |
| `avahi` | Runs the Avahi network service |

**Login Restrictions**

System accounts are generally **not intended for interactive login**. Their login shell is often: `/usr/sbin/nologin` or `/bin/false`. These shells prevent users from logging in to the account normally.
A regular user usually has a real shell: `/bin/bash`.

<i> Note: System accounts exist mainly for: `Service → Process → Limited System User → Required Resources`. They are not regular accounts intended for people. </i>

## Checking and Configuring Hardware

Linux automatically detects and manages most hardware devices.

**Udev**

**Udev** is a Linux subsystem that:

- Detects devices when they are connected or removed.
- Dynamically creates and removes device interfaces.
- Assigns names to devices.

For example:

```text
Connect USB drive
        ↓
Linux kernel detects it
        ↓
Udev identifies the device
        ↓
Device becomes available to the system
```

**Hardware Management Features:**

- **Automatic:** Linux detects most hardware and loads the necessary drivers automatically.
- **Dynamic:** Removable devices can be connected and disconnected while the system is running.
- **Flexible:** Users can configure what happens when a device is connected.

Hardware administration includes:

- Viewing hardware information.
- Managing removable devices.
- Loading and managing hardware drivers.

### Checking the Hardware
