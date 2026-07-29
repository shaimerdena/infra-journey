# Managing User Accounts

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Managing User Accounts](#managing-user-accounts)
  - [Creating User accounts](#creating-user-accounts)
    - [`useradd`](#useradd)
    - [User defaults](#user-defaults)
      - [`/etc/login.defs`](#etclogindefs)
      - [`/etc/default/useradd`](#etcdefaultuseradd)
      - [Changing default settings](#changing-default-settings)
    - [Skeleton directory](#skeleton-directory)

## Creating User accounts
### `useradd`

| Option | Purpose | Example |
|--------|---------|---------|
| `-c "comment"` | Add a description for the user (usually the full name). | `useradd -c "Jake Jackson" jake` |
| `-d <home_dir>` | Set a custom home directory instead of the default `/home/<username>`. | `useradd -d /mnt/homes/jake jake` |
| `-D` | Change the default settings used when creating new users. | `useradd -D` |
| `-e <YYYY-MM-DD>` | Set the account expiration date. | `useradd -e 2029-05-05 jake` |
| `-f <days>` | Set how many days after a password expires before the account is disabled. `-1` means never disable automatically. | `useradd -f 7 jake` |
| `-g <group>` | Set the user's primary group. The group must already exist. | `useradd -g wheel jake` |
| `-G <group1,group2>` | Add the user to one or more supplementary groups. | `useradd -G wheel,sales,tech jake` |
| `-k <skel_dir>` | Use a custom skeleton directory when creating the home directory. Works only with `-m`. | `useradd -m -k /etc/customskel jake` |
| `-m` | Create the user's home directory and copy files from `/etc/skel`. (Default on Fedora/RHEL, not on Ubuntu.) | `useradd -m jake` |
| `-M` | Do not create a home directory. | `useradd -M jake` |
| `-n` | Do not create a new group with the same name as the user. | `useradd -n jake` |
| `-o` | Allow creating a user with a duplicate UID. Must be used with `-u`. | `useradd -o -u 1002 tim` |
| `-p <password>` | Set an encrypted password during account creation. | `useradd -p <encrypted_password> jake` |
| `-s <shell>` | Set the user's default login shell. | `useradd -s /bin/bash jake` |
| `-u <UID>` | Assign a specific user ID instead of using the next available one. | `useradd -u 1793 jake` |

Example:

```bash
$ sudo useradd -c "John Smith" jsmith -g users -G apachi,wheels -s /bin/bash
```

### User defaults

`useradd` reads default settings from: `/etc/login.defs` and `/etc/default/useradd`. These files define how new user accounts are created.

---

#### `/etc/login.defs`

This file contains system-wide defaults, such as:

| Setting | Purpose |
|---------|---------|
| `PASS_MAX_DAYS` | Maximum number of days a password is valid. |
| `PASS_MIN_DAYS` | Minimum number of days before a password can be changed. |
| `PASS_MIN_LEN` | Minimum password length. |
| `PASS_WARN_AGE` | Number of days before password expiration to warn the user. |
| `UID_MIN` / `UID_MAX` | Range of automatically assigned user IDs. |
| `GID_MIN` / `GID_MAX` | Range of automatically assigned group IDs. |
| `CREATE_HOME` | Whether a home directory is created automatically. |

You can change these values by editing the file.

---

#### `/etc/default/useradd`

This file stores default options used by `useradd`.

View current defaults:

```bash
useradd -D
```

Example output:

```text
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/bash
SKEL=/etc/skel
CREATE_MAIL_SPOOL=yes
```

---

#### Changing default settings

Use the `-D` option together with one or more of the following:

| Option | Purpose |
|---------|---------|
| `-b <directory>` | Set the default location for home directories. |
| `-e <YYYY-MM-DD>` | Set the default account expiration date. |
| `-f <days>` | Set the default number of inactive days after password expiration. |
| `-g <group>` | Set the default primary group. |
| `-s <shell>` | Set the default login shell. |

Example:

```bash
useradd -D -b /home/everyone -s /bin/tcsh
```

---

### Skeleton directory

`/etc/skel` contains default files that are copied to a new user's home directory.

Examples:

- `.bashrc`
- `.profile`
- login scripts

---
