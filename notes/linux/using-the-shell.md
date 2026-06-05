# Using the Shell

Table of Contents:
- [Using the Shell](#using-the-shell)
  - [Shell Basics](#shell-basics)
  - [System Information](#system-information)
  - [Viewing the Date and Time](#viewing-the-date-and-time)
  - [Directory Navigation](#directory-navigation)
  - [Locating Commands](#locating-commands)
  - [Types of Commands](#types-of-commands)
  - [Command-line editing](#command-line-editing)
    - [keystrokes for navigating commands:](#keystrokes-for-navigating-commands)
    - [keystrokes for editing commands:](#keystrokes-for-editing-commands)
    - [keystrokes for cutting and pasting:](#keystrokes-for-cutting-and-pasting)
  - [Command-line recall](#command-line-recall)
    - [keystrokes for using command history:](#keystrokes-for-using-command-history)
  - [Connecting and Expanding Commands](#connecting-and-expanding-commands)
    - [Piping between commands](#piping-between-commands)
    - [Sequential commands](#sequential-commands)
    - [Background commands](#background-commands)
    - [Command Substitution](#command-substitution)
    - [Expanding arithmetic expressions](#expanding-arithmetic-expressions)
    - [Expanding variables](#expanding-variables)

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


## Command-line editing

<strong> Use the arrow keys to navigate through the command history. </strong>

### keystrokes for navigating commands:
| Key | Description |
| --- | ----------- |
| Up Arrow | Move to the previous command in the history |
| Down Arrow | Move to the next command in the history |
| Left Arrow | Move the cursor left within the current command |
| Right Arrow | Move the cursor right within the current command |
| Ctrl + A | Move the cursor to the beginning of the line |
| Ctrl + E | Move the cursor to the end of the line |
| Ctrl + L | Clear the screen and move the cursor to the top |
| Alt + F | Move the cursor forward by one word |
| Alt + B | Move the cursor backward by one word |

### keystrokes for editing commands:
| Key | Description |
| --- | ----------- |
| Ctrl + D | Delete the character under the cursor |
| Ctrl + H (backspace) | Delete the character before the cursor |
| Ctrl + T | Transpose the character under the cursor with the previous character |
| Alt + T | Transpose the current word with the previous word |
| Alt + U | Convert the current word to uppercase |
| Alt + L | Convert the current word to lowercase |
| Alt + C | Capitalize the current word (first letter uppercase, rest lowercase) |
| Ctrl + V | Insert a literal character (useful for inserting control characters) |

### keystrokes for cutting and pasting:
| Key | Description |
| --- | ----------- |
| Ctrl + K | Cut the text from the cursor to the end of the line |
| Ctrl + U | Cut the text from the cursor to the beginning of the line |
| Ctrl + W | Cut the word before the cursor |
| Ctrl + Y | Paste the most recently cut text at the cursor position |
| Ctrl + C | Cancel the current command and return to a new prompt |
| Alt + Y | Paste the previously cut text (cycle through the cut text history) |
| Alt + D | Cut the word after the cursor |

<i> Note: these keystrokes are based on the default keybindings for the Bash shell. Other shells may have different keybindings, and some may allow you to customize them. </i>

<strong> command-line completion </strong>
- Pressing the Tab key while typing a command will attempt to auto-complete the command or file name based on the current context. If there are multiple possible completions, pressing Tab twice will show a list of possible completions. This can save time and reduce errors when typing long commands or file names. For example, if you type `ls /usr/bi` and press Tab, it will auto-complete to `ls /usr/bin/` if there is only one match.

Some of the values that can be auto-completed include:
- Command, alias and function names (e.g., `ls`, `cd`, `myalias`, `myfunction`) 
- Variable names (e.g., `$HOME`, `$PATH`)
- File and directory names (e.g., `myfile.txt`, `mydir/`)
- Hostnames (e.g., when using SSH or SCP)
- Usernames (e.g., when using `su`, `sudo`, or ~ for home directory)

## Command-line recall

<strong> Use the `history` command to view your command history. </strong>
``` bash
$ history
``` 

<strong> add a number to the `history` command to show only the most recent commands. </strong>
``` bash
$ history NUMBER
```

<strong> Several ways to recall and execute previous commands: </strong>
- `!n` - Execute the command with the specified history number (e.g., `!42` will execute the command numbered 42 in the history list).
- `!!` - Execute the previous command (e.g., if you just ran `ls`, typing `!!` will run `ls` again).
- `!string` - Execute the most recent command that starts with the specified string (e.g., `!ls` will execute the most recent command that starts with `ls`).
- `!?string?` - Execute the most recent command that contains the specified string (e.g., `!?git?` will execute the most recent command that contains `git` anywhere in the command).
- `^old^new` - Replace the first occurrence of `old` with `new` in the previous command and execute it (e.g., if you just ran `git commit -m "old message"`, typing `^old^new` will run `git commit -m "new message"`).

### keystrokes for using command history:
| Key | Description |
| --- | ----------- |
| Arrow keys | Navigate through command history |
| Ctrl + R | Search command history interactively |
| Ctrl + S | Search command history forward (if supported) |
| Ctrl + G | Exit history search mode |

<i> Note: another way of working with command history is to use the `fc` command, which allows you to edit and re-execute commands from your history using a text editor. For example, `fc -l` will list your command history with line numbers, and `fc -e nano 42` will open the command numbered 42 in the nano text editor for editing before executing it. </i>

<i> Note: to disable command history, you can set the `HISTFILE` variable to an empty string: </i>
``` bash
$ export HISTFILE=""
```

<i> this will prevent the shell from saving your command history to a file, effectively disabling it. </i>

## Connecting and Expanding Commands

- metacharacter - a character that has a special meaning to the shell and is used to perform specific functions, such as connecting commands or redirecting input/output. Examples include `|`, `&&`, `||`, `;`, `>`, `<`, and more.

### Piping between commands

<strong> The pipe (|) metacharacter connects the output from one command to the input of another command. </strong>

<i> Example: </i>
```bash
$ cat /etc/passwd | sort | less
```

<i> This command lists the contents of the `/etc/passwd` file and pipes the output to the `sort` command. The `sort` command takes the usernames that begin each line of the `/etc/passwd` file, sorts them alphabetically, and pipes the output to the `less` command (to page through the output) </i>

<i> Note: Pipes are an excellent illustration of how UNIX was created as an OS made up of building blocks. </i>

### Sequential commands

<strong> To run a sequence of several commands on the same command line, separate commands with semicolons (;).</strong>

<i> Example: </i>
```bash
$ date ; troff -me verylargedocument | lpr ; date
```

### Background commands

<strong> By default, commands run in the foreground and occupy the terminal. Run a command in the background using &. Syntax: `$ command &` </strong>

<i> Example: </i>
```bash
$ sleep 30 &
```

<strong>Useful commands: </strong>
- jobs - to list background jobs
- fg - to bring job to foreground

### Command Substitution

<strong> Command substitution executes a command and replaces it with its output. Syntax: `$(command)`</strong>

<i> Examples: </i>

```bash
$ echo $(date)

$ echo $(whoami)

$ mkdir backup-$(date +%Y-%m-%d)
```

<i> Note: difference from pipes
- Pipe (|): output of one command becomes input of another command.
- Command substitution ($()): output of one command becomes an argument of another command.
</i>

### Expanding arithmetic expressions

<strong> There are two forms that you can use to expand an arithmetic expression and pass it to the shell: $[expression] or $(expression). </strong>

<i> Example: </i>

```bash
$ echo "I am $[2026 - 2006] years old. "
I am 20 years old.
$ echo "There are $(ls | wc -w) files in this directory."
There are 3 diles in this directory.
```

<i> wc -w - to count the number of files found </i> <br>
<i> wc - to count lines, words, characters, and bytes in files or from piped input </i>

### Expanding variables

<strong> Variables that store information within the shell can be expanded using the dollar sign. Syntax: `$VARIABLE`</strong> <br>

<i>Example:</i>
```bash
$ echo $HOME
/home/user
```
 
<i>Example:</i>
```bash
$ ls -l $BASH
```

Bash expands `$BASH` to `/usr/bin/bash`, so the resulting command:

```bash
$ ls -l /usr/bin/bash
```