# Using the Shell

## Shell Basics

<strong> command structure </strong>
``` bash
$ command [options] [arguments]
```

- the default prompt for a regular user is - $
- the default prompt for the root user is - #

<i>Note: although we use # to indicate that a command be run as the root user, you do not need to log in as the root user to run these commands. You can use the sudo command to run commands with elevated privileges.</i>

<strong> command structure breakdown </strong>
```bash
username@hostname:~/current/directory$ command [options] [arguments]
```
<i> to use a whole word as an option, we can use double dashes -- </i>
```bash
$ command --option
```

<i> to use a single letter as an option, we can use a single dash - </i>
```bash
$ command -o
```

<i> with single-letter options, the argument typically follows the option, but with whole-word options, the argument can be separated by a space or an equals sign </i>
```bash
$ command -o argument
$ command --option=argument
```

<i> Note: you can use the --help option with most commands to get a summary of how to use the command, including available options and arguments. </i>
```bash
$ command --help
```


## System Information

| Command    | Description                                   | Results                                      |
| ---------- | --------------------------------------------- | -------------------------------------------- |
| `uname -a` | Show system information                      | Kernel name, hostname, kernel version
| `id` | Show user and group information | uid=1000(USERNAME) gid=1000(USERNAME) groups=1000(USERNAME),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)
| `who` | Show who is logged in                      | USERNAME pts/0 2023-03-15 14:30 (192.168.1.100)
| `w` | Show who is logged in and what they are doing | 14:30:00 up 1 day, 2:30, 3 users, load average: 0.00, 0.01, 0.05
| `who -u` | Show who is logged in with idle time | USERNAME pts/0 2023-03-15 14:30 |
| `who -H` | Show who is logged in with headers | USERNAME TTY FROM LOGIN@ IDLE JCPU PCPU WHAT |
| `lsb_release -a` | Show Linux distribution information | Distributor ID, description, release, codename |
| `hostname` | Show the hostname of the system | myhostname |
| `uptime` | Show how long the system has been running | 14:30:00 up 1 day, 2:30, 3 users, load average: 0.00, 0.01, 0.05
| `whoami` | Show the current logged in user | USERNAME |
| `grep $USER /etc/passwd` | Show user account information | USERNAME:x:1000:1000:Full Name,,,:/home/USERNAME:/bin/bash
| `cat /etc/shells` | Show available shells | /bin/sh /bin/bash /bin/rbash /bin/dash /bin/zsh |
| `man SHELL` | Show manual page for default shell | Manual page for the default shell |
| `man COMMAND` | Show manual page for a specific command | Manual page for the specified command |
| `man SECTION_NUMBER COMMAND` | Show manual page for a specific command in a specific section | Manual page for the specified command in the specified section |

<i> -a - shows all available information </i> <br>

<i> whoami - shows the current logged in user </i> <br>
<i> grep - searches for the specified pattern in the file </i> <br>
<i> /etc/passwd - a file that contains user account information </i> <br>
<i> bin/bash - indicates that the default login shell is bash </i> <br>

<i> cat - displays the contents of the file </i> <br>
<i> /etc/shells - a file that lists the available shells on the system </i> <br>

<i> man - displays the manual page for the specified command </i> <br>

## Viewing the Date and Time

| Command    | Description                                   | Results                                      |
| ---------- | --------------------------------------------- | -------------------------------------------- |
| `date`     | Show current date and time                   | Day Month Date Time Timezone Year           |
| `date -u`  | Show current date and time in UTC            | Day Month Date Time Timezone Year           |
| `date +"%Y-%m-%d %H:%M:%S"` | Show current date and time in specific format | 2023-03-15 14:30:00                        |

<i> %Y - year, %m - month, %d - day, %H - hour, %M - minute, %S - second </i> <br>

## Directory Navigation

| Command    | Description                     | Results                          |
| ---------- | ------------------------------- | -------------------------------- |
| `pwd`      | Show current directory          | /home/USERNAME                   |
| `pwd -P`   | Show physical path (no symlink) | /home/USERNAME                   |
| `cd <dir>` | Change directory                | /home/USERNAME/<dir>             |
| `cd ..`    | Move to parent directory        | /home/USERNAME                   |
| `cd -`     | Go to previous directory        | /home/USERNAME                   |
| `cd ~`     | Go to home directory            | /home/USERNAME                   |
| `cd /`     | Go to root directory            | /                                |
| `cd /path/to/dir` | Go to specific directory | /path/to/dir |
| `ls`       | List directory contents         | file1.txt file2.txt dir1/        |
| `ls -l`    | List directory contents in long format | -rw-r--r-- 1 USERNAME USERNAME 1234 Mar 15 14:30 file1.txt |
| `ls -a`    | List all directory contents (including hidden files) | . .. .hiddenfile file1.txt file2.txt dir1/ |
| `ls -t` | List directory contents sorted by modification time | file2.txt file1.txt dir1/ |
| `ls --hide=PATTERN` | Hide files matching the specified pattern | file1.txt dir1/ |

<strong> we can combine options </strong>
``` bash
$ ls -lat
```
- this command will list all directory contents in long format, including hidden files, and sorted by modification time.
- the order of options does not matter, so `ls -lat` and `ls -atl` will produce the same result.

## Locating Commands

<strong> We can use the `which` command to find the location of a command in the system's PATH. </strong>
``` bash
$ which command
```
- this will return the full path to the command if it is found in the PATH, or nothing if it is not found.

<i> Note: commands are located in directories that are listed in the PATH environment variable. You can view the PATH variable using the `echo` command: </i>
``` bash
$ echo $PATH
```
- this will display a colon-separated list of directories that the shell searches for commands. When you enter a command, the shell looks through these directories in order to find the executable file for that command. If the command is found, it is executed; if not, you will get a "command not found" error.

<i> Note: Most user commands that come with Linux are stored in the `/usr/bin` or `/usr/local/bin` directory. The `/sbin` and `/usr/sbin` directories typically contain system administration commands. </i>
<i> You can add your own custom scripts to a directory that is in your PATH, such as `/usr/local/bin`, to make them easily accessible from the command line to all users on your system, or `/home/user/bin` to create a personal bin directory for your scripts. </i>

## Types of Commands

<strong>Command Lookup Order</strong><br>
<i>When a command is entered, the shell searches in the following order:</i>
1. Alias
2. Function
3. Shell Built-in
4. Executable file in PATH

| Command Type | Description | Example |
| ------------ | ----------- | ------- |
| Built-in     | Commands that are built into the shell and do not require an external executable | `cd`, `pwd`, `echo`, `exit`, `history`, `fg`, `set` |
| External (filesystem)    | Commands that are separate executable files located in the filesystem | `ls`, `grep`, `cat`, `find` |
| Alias         | A shortcut for a command or a series of commands | `alias gs='git status'` |
| Function      | A block of code that can be executed by calling its name | `my_function() { echo "Hello, World!"; }` |

<i> echo - to display a line of text or a variable value </i> <br>
<i> exit - to exit the shell or a script </i> <br>
<i> fg - to bring a command running in the background to the foreground </i> <br>
<i> history - to display the command history </i> <br>
<i> set - to set shell options and variables </i> <br>

Results of the `type` command:
- If the command is a built-in: `command is a shell builtin`
- If the command is an external executable: `command is /path/to/command`
- If the command is an alias: `command is aliased to 'actual_command'`
- If the command is a function: `command is a function`
- If the command is not found: `command not found`

<i> Note: the are also shell reserved words that are not commands and cannot be overridden by aliases or functions. These include words like `if`, `then`, `else`, `fi`, `for`, `while`, `do`, `done`, `case`, `esac`, `function`, and more. </i>

<strong> to check if a command is a built-in, external, alias, function, or reserved, you can use the `type` command: </strong>
``` bash
$ type command
```

<i> Note: An external command can be wrapped by an alias. <br>
Example: alias ls='ls --color=auto' <br>
The alias is expanded first, then the actual executable file (/usr/bin/ls) is executed.</i>

<strong> to check all occurrences of a command in the lookup order, you can use the `-a` option with the `type` command: </strong>
```bash
$ type -a command
```

<strong> if a command is not in your PATH, you can use the `plocate` command to find its location: </strong>
```bash
$ plocate command
```

<i> this will search the file system for the specified command and return its location if found. Note that `plocate` is a faster alternative to the `locate` command, as it uses a pre-built database of file locations instead of searching the file system in real-time. </i>