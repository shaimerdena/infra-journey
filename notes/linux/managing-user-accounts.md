# Managing User Accounts

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Managing User Accounts](#managing-user-accounts)
  - [`useradd`](#useradd)

## `useradd`

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