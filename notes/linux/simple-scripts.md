# Simple Scripts

> Notes based on Linux Bible and personal learning experiments.

Table of Contents:
- [Simple Scripts](#simple-scripts)
  - [Understanding Shell Scripts](#understanding-shell-scripts)
    - [Executing and Debugging shell scripts](#executing-and-debugging-shell-scripts)
      - [Ways to Execute a Script](#ways-to-execute-a-script)
      - [Command-Line Arguments](#command-line-arguments)
      - [Comments](#comments)
      - [Debugging Techniques](#debugging-techniques)
      - [Good Practices](#good-practices)
    - [Understanding Shell Variables](#understanding-shell-variables)
      - [Command Substitution](#command-substitution)
      - [Escaping Special Characters](#escaping-special-characters)
      - [Quotes](#quotes)

## Understanding Shell Scripts

<strong> A shell script is a text file containing a sequence of shell commands that can be executed automatically. </strong>

Shell scripts are similar to Windows batch files. They can contain: commands, variables, loops, conditions, functions, arithmetic operations. Shell scripts can automate repetitive tasks.

### Executing and Debugging shell scripts

#### Ways to Execute a Script

1. Run Through Bash

```bash
bash myscript
```

Requirements:

- script does not need execute permission
- Bash interprets the file directly

---

2. Run as a Program

Add a shebang (#!) at the top:

```bash
#!/bin/bash
```
The first line of a script can specify which interpreter should run it. This tells Linux: Use Bash to execute this script.

Make the file executable:

```bash
chmod +x myscript
```

Run it:

```bash
./myscript
```

---

#### Command-Line Arguments

Anything after the script name is an argument.

Example:

```bash
./myscript file.txt
```

Here `file.txt` is a command-line argument.

---

#### Comments

Use `#` for comments.

```bash
echo "Hello"  # Inline comment
```

---

#### Debugging Techniques

1. Use `echo`

```bash
echo "Processing file..."
```

Allows you to verify program flow.

---

2. Show Executed Commands

```bash
bash -x myscript
```

or inside the script:

```bash
set -x
```

Display each command before execution.

Useful for troubleshooting.

---

#### Good Practices

- Write scripts in small stages
- Test frequently
- Add comments
- Keep logic simple and readable
- Use `echo` for debugging

---

### Understanding Shell Variables

<strong> Variables store data that can be reused in a shell script. </strong>

---

<strong> Creating Variables </strong>

Syntax:

```bash
$ NAME=value
$ CITY="Springfield"
```

---

#### Command Substitution

A variable can store the output of a command.

```bash
MYDATE=$(date)
```

Equivalent:

```bash
MYDATE=`date`
```

Example:

```bash
echo $MYDATE
```

Displays the saved output of the `date` command.

---

#### Escaping Special Characters

Some characters have special meaning in Bash:

```text
$  `  *  !
```

Use a backslash (`\`) to treat a character literally:

```bash
echo \$HOME
```

Output:

```text
$HOME
```

---

#### Quotes

| Quotes | Behavior |
|----------|----------|
| `' '` | Everything is treated literally |
| `" "` | Variables and commands are expanded |
| No quotes | Shell expands variables, wildcards, commands |

<i> Examples </i>

- `echo '$HOME'`: $HOME
- `echo "$HOME"`: /home/user
- `echo $HOME`: /home/user

