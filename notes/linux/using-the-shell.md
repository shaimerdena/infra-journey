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

<i> Note: you can use the --help option with most commands to get a summary of how to use the command, including available options and arguments. </i>
```bash
$ command --help
```


## System Information

| Command    | Description                                   | Results                                      |
| ---------- | --------------------------------------------- | -------------------------------------------- |
| `uname -a` | Show system information                      | Kernel name, hostname, kernel version
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

<strong> we can combine options </strong>
``` bash
$ ls -lat
```
- this command will list all directory contents in long format, including hidden files, and sorted by modification time.
- the order of options does not matter, so `ls -lat` and `ls -atl` will produce the same result.

