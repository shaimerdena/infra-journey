# Managing Running Processes

Table of Contents:
- [Managing Running Processes](#managing-running-processes)
  - [Understanding Processes](#understanding-processes)
  - [Listing Processes](#listing-processes)
    - [`ps`](#ps)
    - [Useful `ps` Options](#useful-ps-options)
    - [Background Processes](#background-processes)
    - [`procs`](#procs)
    - [`top`](#top)
      - [Renice a Process](#renice-a-process)
      - [Kill a Process](#kill-a-process)

## Understanding Processes

<strong> A process is a running instance of a command or program. </strong>

<strong> Key Concepts </strong>

- Every running program is a process.
- Multiple users can run the same command, creating multiple processes.
- Each process has a unique **Process ID (PID)** while it is running.
- After a process ends, its PID may be reused by another process.

Each process is associated with:

| Attribute | Description |
|------------|------------|
| PID | Unique process identifier |
| User | Owner of the process |
| Group | Group associated with the process |

These attributes determine what resources the process can access.

<i> Example </i>

One command can have many running processes:

```text
vi command
├── PID 1201 (user: cipher)
├── PID 1354 (user: alice)
└── PID 1420 (user: bob)
```

---

Permissions and Processes

- A process running as `root` has extensive system access.
- A process running as a regular user has limited permissions.
- Process permissions depend on the user account that started the process.

---

Why Process Management Matters

System administrators monitor processes to:

- identify high CPU usage
- identify excessive memory consumption
- detect runaway or frozen processes
- manage system performance

---

Process Information and `/proc`

Linux stores process information in `/proc`. Each running process has its own directory `/proc/<PID>`. Process-monitoring commands often obtain their information from the `/proc` filesystem.

## Listing Processes

<strong> Linux provides several tools for viewing and managing running processes. </strong>

<i> Common Process Tools </i>

| Command | Purpose |
|-----------|----------|
| `ps` | Display running processes |
| `top` | Real-time process monitoring |
| `procs` | Modern alternative to `ps` |
| `gnome-system-monitor` | Graphical process manager (GNOME) |

---

`ps`

- Traditional command for listing processes.
- Shows information about currently running processes.
- Supports many options inherited from UNIX and BSD systems.
- Often used in scripts and administration tasks.

---

`procs`

- Modern replacement for `ps`.
- Provides cleaner and more readable output.
- Easier to use for everyday process inspection.

---

`top`

- Displays processes in real time.
- Continuously updates CPU and memory usage.
- Can be used to monitor system performance.
- Allows interaction with running processes.

---

 GNOME System Monitor

- Graphical tool for viewing processes.
- Shows CPU, memory, and resource usage.
- Allows users to stop or manage processes through a GUI.

<i> Notes:

```text
ps    → snapshot of processes
top   → live monitoring
procs → modern ps
GUI   → gnome-system-monitor 
```
</i>

### `ps`

<strong> The `ps` command displays information about currently running processes. </strong>

Basic Usage

```bash
ps u
```

Displays processes for the current user with additional details such as CPU usage, memory usage, and start time.

---

Common Columns

| Column | Description |
|----------|----------|
| `USER` | User who started the process |
| `PID` | Process ID |
| `%CPU` | CPU usage percentage |
| `%MEM` | Memory usage percentage |
| `VSZ` | Virtual memory allocated (KB) |
| `RSS` | Physical memory currently used (KB) |
| `TTY` | Terminal associated with the process |
| `STAT` | Current process state |
| `START` | Process start time |
| `TIME` | Total CPU time consumed |
| `COMMAND` | Command that started the process |

---

<strong> Understanding `TTY` </strong>

| Type | Description |
|--------|------------|
| `tty` | Physical or login terminal |
| `pts` | Pseudo-terminal (Terminal window, SSH session, etc.) |

Example:

```text
pts/2
```

means the process is running inside a terminal session.

---
 
<strong> Common Process States (`STAT`) </strong>

| State | Meaning |
|----------|----------|
| `R` | Running |
| `S` | Sleeping (waiting for work) |
| `s` | Session leader |
| `l` | Multi-threaded process |
| `+` | Foreground process |

Example:

```text
R+
```

A running process in the foreground.

---

<strong> Memory Information </strong>

| Column | Meaning |
|----------|----------|
| `VSZ` | Total virtual memory allocated |
| `RSS` | Physical memory currently in RAM |

`VSZ` can be larger than `RSS` because not all allocated memory is actively used.

---

</i> Notes:

```text
ps u     → process information for current user

PID      → process identifier
TTY      → terminal/session
STAT     → process state

R        → running
S        → sleeping
```
</i>

### Useful `ps` Options

| Command / Option | Description |
|------------------|-------------|
| `ps u` | Show processes for the current user with detailed information |
| `ps ux` | Show all processes for the current user |
| `ps aux` | Show all processes for all users |
| `ps -e` | Show every running process |
| `ps -o` | Display selected columns |
| `ps -eo ...` | Show all processes with custom columns |
| `--sort=field` | Sort output by a column |
| `--sort=-field` | Sort in descending order |

---

<i> Examples </i>

Custom Output:

```bash
ps -eo pid,user,uid,group,gid,vsz,rss,comm
```

Sort by Virtual Memory Usage:

```bash
ps -eo pid,user,vsz,rss,comm --sort=-vsz
```

Show Top Memory Consumers:

```bash
ps -eo pid,user,vsz,rss,comm --sort=-vsz | head
```

### Background Processes

Many Linux processes run without a visible terminal.

Examples:

- logging services
- networking services
- desktop services
- audio services
- authentication services

These processes often start during boot and continue running until shutdown.

### `procs`

<strong> `procs` is a modern alternative to the `ps` command with cleaner and more readable output. </strong>

Basic Usage

| Command | Description |
|----------|----------|
| `procs` | Display all running processes |
| `procs -w` | Watch processes in real time |
| `procs --insert FIELD` | Add a custom column |

---

Useful Fields

| Field | Description |
|----------|----------|
| `TcpPort` | Network port used by the process |
| `Processor` | CPU core running the process |

<i> Example </i>

```bash
procs --insert TcpPort --insert Processor
```

Display additional information about ports and CPU cores.

---

Watch Mode Controls

| Key | Action |
|--------|---------|
| `a` | Sort ascending |
| `d` | Sort descending |
| `n` | Next page |
| `p` | Previous page |

### `top`

<strong> The `top` command provides a real-time view of running processes and system resource usage. </strong>

Basic Usage

```bash
top
```

Displays:

- running processes
- CPU usage
- memory usage
- swap usage
- system uptime
- logged-in users
- system load

---

<strong> Useful Controls </strong>

| Key | Action |
|------|---------|
| `h` | Show help |
| `M` | Sort by memory usage |
| `P` | Sort by CPU usage |
| `R` | Reverse sort order |
| `1` | Show usage for each CPU core |
| `u` | Filter by username |
| `q` | Quit `top` |

---

#### Renice a Process

| Key | Action |
|------|---------|
| `r` | Change process priority |

Steps:

1. Press `r`
2. Enter PID
3. Enter priority value (`-20` to `19`)

```text
-20 → highest priority
 19 → lowest priority
```

---

#### Kill a Process

| Key | Action |
|------|---------|
| `k` | Send signal to a process |

Steps:

1. Press `k`
2. Enter PID
3. Enter signal number

Common signals:

| Signal | Meaning |
|----------|----------|
| `15` | Graceful termination (SIGTERM) |
| `9` | Force kill (SIGKILL) |