# Filesystem

> Notes based on Linux Bible and personal learning experiments.

Table of contents: 
- [Filesystem](#filesystem)
  - [Linux Directories](#linux-directories)
  - [Linux vs Windows Filesystems](#linux-vs-windows-filesystems)
  - [Commands to create and use files](#commands-to-create-and-use-files)
  - [Using metacharacters and operators](#using-metacharacters-and-operators)
    - [File-matching metacharacters](#file-matching-metacharacters)
    - [File-redirection metacharacters](#file-redirection-metacharacters)
    - [Brace expansion characters](#brace-expansion-characters)
  - [Listing Files and Directories](#listing-files-and-directories)
    - [Understanding `ls -l`](#understanding-ls--l)
    - [Understanding `ls -la`](#understanding-ls--la)
    - [Special Permissions](#special-permissions)
      - [SetUID / SetGID (`s`)](#setuid--setgid-s)
      - [Sticky Bit (`t`)](#sticky-bit-t)
      - [Additional Indicators](#additional-indicators)

## Linux Directories

| Directory | Purpose |
|------------|----------|
| `/` | Root directory (top of the filesystem) |
| `/bin` | Essential user commands (`ls`, `cp`, `cat`, `mkdir`) |
| `/boot` | Files required to boot Linux |
| `/dev` | Device files (disks, terminals, USB devices) |
| `/etc` | System configuration files |
| `/home` | Home directories of regular users |
| `/lib` | Shared libraries required by programs |
| `/media` | Automatically mounted removable devices (USB drives, CDs) |
| `/mnt` | Temporary mount point for filesystems |
| `/opt` | Optional third-party software |
| `/proc` | Information about processes and the Linux kernel |
| `/root` | Home directory of the root user |
| `/sbin` | System administration commands |
| `/sys` | Kernel and hardware information |
| `/tmp` | Temporary files |
| `/usr` | User applications, libraries, and documentation |
| `/var` | Variable data (logs, web files, mail, databases) |

## Linux vs Windows Filesystems

| Feature | Linux | Windows |
|----------|--------|---------|
| Storage organization | Single filesystem tree (`/`) | Multiple drives (`C:`, `D:`) |
| Path separator | `/` | `\` |
| Executable files | Determined by permissions (`chmod +x`) | Often determined by extensions (`.exe`, `.bat`) |
| File ownership | Every file has an owner and permissions | Added later; originally designed as a single-user system |

<i> Example Paths </i>

Linux: `/home/cipher/Documents` <br>
Windows: `C:\Users\Cipher\Documents`

## Commands to create and use files

Basic Filesystem Commands

| Command |	Description |
|----------|--------|
| cd | 	Navigate to a different directory
| pwd |	Display the current working directory
| mkdir |	Create a new directory
| chmod | 	Modify file or directory permissions
| ls |	Show the contents of a directory


<i> Notes: 
* Running cd without arguments returns you to your home directory.
* Use pwd to verify your current location in the filesystem.
* The mkdir command is used to create new directories.
* File and directory permissions can be modified with chmod. </i>

To view permissions and details of a directory:
```bash
$ ls -ld test
drwxr-xr-x 2 cipher users 4096 Jun 10 12:00 test
```
<i> ls -l - displays detailed info</i> <br>
<i> ls -d - only lists the name of directories </i>

## Using metacharacters and operators

### File-matching metacharacters

<strong> Bash supports wildcard characters (globbing) to match multiple files. </strong>

| Metacharacter | Description |
|---------------|-------------|
| `*` | Matches zero or more characters |
| `?` | Matches exactly one character |
| `[abc]` | Matches one character from the specified set |
| `[a-z]` | Matches one character within a range |

<i>Examples:</i>

- Matches files starting with `a`: `ls a*`
- Matches files containing `e`: `ls *e*`
- Matches files starting with `g` and ending with `t`: `ls g*t`
- Matches five-character filenames ending with `e`: `ls ????e`
- Matches files starting with `a`, `b`, or `w`: `ls [abw]*`
- Matches files starting with letters from `a` to `g`: `ls [a-g]*`

<i> Notes

- Wildcards are expanded by the shell before the command is executed.
- They can be used with many commands such as `ls`, `cp`, `mv`, and `rm`.
- Wildcards help work with multiple files without specifying each filename manually. </i>

### File-redirection metacharacters

<strong> Linux commands receive input from <em>standard input (stdin)</em> and send results to <em>standard output (stdout)</em>. </strong>

<strong> Redirection Operators </strong>

| Operator | Description |
|-----------|-------------|
| `<` | Read input from a file |
| `>` | Write output to a file (overwrite) |
| `>>` | Append output to a file |
| `2>` | Redirect error messages (stderr) to a file |
| `&>` | Redirect both output and errors to a file |
| pipe | Send output of one command as input to another |

`|`   command → command <br>
`>`   output → file <br>
`>>`  output → append to file <br>
`<`   file → command <br>
`2>`  errors → file <br>
`&>`  everything → file <br>
 
<i> Examples </i>

- Read a file as input: `sort < names.txt` (it is the same as using `sort names.txt`)
- Write output to a file: `ls > files.txt`
- Append output to a file: `date >> log.txt`
- Save errors only: `find /root 2> errors.txt`
- Save both output and errors: `find / &> output.log`
- Use a pipe: `ls | grep txt`

<strong> Here Documents </strong>

A here document (`<<`) allows multiline input to be passed to a command.

Example:

```bash
cat << EOF
Hello
Linux
EOF
```

Output:

```text
Hello
Linux
```

### Brace expansion characters

<strong> Brace expansion (`{ }`) generates multiple strings from a single pattern. Syntax: `{item1, item2, item3}`, `start..end`.</strong>

<i>Example: </i>

- Create multiple directories: `mkdir project-{frontend,backend,database}`. Result:

```text
project-frontend
project-backend
project-database
```

- Create multiple files using a range: `touch log{1..4}.txt`. Result:

```text
log1.txt
log2.txt
log3.txt
log4.txt
```
- Combine multiple expansions: `touch env-{dev,test,prod}-{1..2}.conf`. Result:

```text
env-dev-1.conf
env-dev-2.conf
env-test-1.conf
env-test-2.conf
env-prod-1.conf
env-prod-2.conf
```

<i> Notes: 
- Brace expansion occurs before command execution.
- Supports both comma-separated values and ranges.
- Can be combined with other shell features. </i>

## Listing Files and Directories

### Understanding `ls -l`

<strong> The first character in `ls -l` output indicates the file type. </strong>

| Symbol | Type |
|----------|----------|
| `-` | Regular file |
| `d` | Directory |
| `l` | Symbolic link |

<i>Examples </i>

* Regular file - `-rw-r--r-- file.txt`
* Directory - `drwxr-xr-x Documents`
* Symbolic link - `lrwxrwxrwx config -> /etc/config`

<strong> An executable file has the `x` permission set. The `x` bits indicate that the file can be executed as a program or script. </strong>

<i> Example: </i> `-rwxr-xr-x script.sh`

### Understanding `ls -la`

<strong> The `ls -la` command displays a detailed listing of files and directories, including hidden files. </strong>

| Option | Description |
|---------|-------------|
| `-l` | Long listing format |
| `-a` | Show hidden files (files starting with `.`) |

<strong> Files and directories beginning with `.` are hidden by default. These files are commonly used for shell configuration and user settings. </strong>

Examples:

```text
.bashrc
.bash_profile
.bash_history
```

<strong> Special Directories </strong>

| Entry | Meaning |
|---------|----------|
| `.` | Current directory |
| `..` | Parent directory |

<i>Example: </i> 
```bash
$ ls -la
total 8
drwxrwxr-x 2 cipher cipher 4096 Jun 10 15:30 .
drwxr-­x--x 7 cipher cipher 4096 Jun 10 15:30 ..
-rw-rw-r--  1 cipher cipher 0 Jun 10 15:30 letter
```
<i> total - the total amount of disk space used by the files. </i>

| Column | Description |
|----------|-------------|
| 1 | File type and permissions |
| 2 | Number of hard links |
| 3 | Owner |
| 4 | Group |
| 5 | Size (bytes) |
| 6 | Last modification date |
| 7 | File or directory name |

### Special Permissions

#### SetUID / SetGID (`s`)

<strong> Programs with the `s` bit run with the privileges of the file owner or group.</strong>

Example:

```text
-rwsr-xr-x
```

#### Sticky Bit (`t`)

<strong> Commonly used on shared directories. Users can create files but cannot delete files owned by others. </strong>

Example:

```text
drwxrwxrwt
```

#### Additional Indicators

| Symbol | Meaning |
|----------|----------|
| `+` | ACLs (Access Control Lists) are configured |
| `.` | SELinux context is applied |