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
  - [Understanding Shell Variables](#understanding-shell-variables)
    - [Special Shell Positional Parameters](#special-shell-positional-parameters)
    - [Reading User Input](#reading-user-input)
    - [Parameter Expansion in Bash](#parameter-expansion-in-bash)
  - [Performing arithmetic in shell scripts](#performing-arithmetic-in-shell-scripts)
    - [Arithmetic with `let`](#arithmetic-with-let)
    - [Arithmetic with `expr`](#arithmetic-with-expr)
    - [Arithmetic with `bc`](#arithmetic-with-bc)
    - [Random Numbers](#random-numbers)
    - [Incrementing Variables](#incrementing-variables)
  - [Programming contructs](#programming-contructs)
    - [Bash `if` Statements](#bash-if-statements)
      - [if...else](#ifelse)
      - [if...elif...else](#ifelifelse)
    - [Bash Test Operators](#bash-test-operators)
      - [File Tests](#file-tests)
      - [String Tests](#string-tests)
      - [Numeric Comparisons](#numeric-comparisons)
      - [Logical Operators](#logical-operators)
      - [File Time Comparisons](#file-time-comparisons)
      - [Rarely Used (System Administration)](#rarely-used-system-administration)
    - [Bash Shortcuts for `if`](#bash-shortcuts-for-if)
      - [Execute if FALSE (`||`)](#execute-if-false-)
      - [Execute if TRUE (`&&`)](#execute-if-true-)
      - [One-line if...else (`&&` + `||`)](#one-line-ifelse---)
    - [`case` command](#case-command)
    - [`for...do` Loop](#fordo-loop)
      - [General Syntax](#general-syntax)
      - [C-style for Loop](#c-style-for-loop)
    - [`while...do` and `until...do` Loops](#whiledo-and-untildo-loops)
      - [`while` Loop](#while-loop)
      - [`until` Loop](#until-loop)
    - [Comparison](#comparison)

## Understanding Shell Scripts

<strong> A shell script is a text file containing a sequence of shell commands that can be executed automatically. </strong>

Shell scripts are similar to Windows batch files. They can contain: commands, variables, loops, conditions, functions, arithmetic operations. Shell scripts can automate repetitive tasks.

<strong> Good Practices </strong>

- Write scripts in small stages
- Test frequently
- Add comments
- Keep logic simple and readable
- Use `echo` for debugging

## Executing and Debugging shell scripts

### Ways to Execute a Script

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

### Command-Line Arguments 

Anything after the script name is an argument.

Example:

```bash
./myscript file.txt
```

Here `file.txt` is a command-line argument.

---

### Comments 

Use `#` for comments.

```bash
echo "Hello"  # Inline comment
```

---

### Debugging Techniques

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

## Understanding Shell Variables

<strong> Variables store data that can be reused in a shell script. </strong>

---

<strong> Creating Variables </strong>

Syntax:

```bash
$ NAME=value
$ CITY="Springfield"
```

---

<strong> Command Substitution </strong>

A variable can store the output of a command.

<i>Example: </i>

`MYDATE=$(date)` is equivalent to `MYDATE=`date`.

---

<strong> Escaping Special Characters </strong>

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
 
<strong> Quotes </strong>

| Quotes | Behavior |
|----------|----------|
| `' '` | Everything is treated literally |
| `" "` | Variables and commands are expanded |
| No quotes | Shell expands variables, wildcards, commands |

<i> Examples </i>

- `echo '$HOME'`: $HOME
- `echo "$HOME"`: /home/user
- `echo $HOME`: /home/user

### Special Shell Positional Parameters

<strong> Positional parameters store command-line arguments passed to a script. </strong>

| Variable | Meaning |
|----------|----------|
| `$0` | Script name (or path used to run it) |
| `$1` | First argument |
| `$2` | Second argument |
| `$3...$n` | Next arguments |
| `$#` | Number of arguments |
| `$@` | All arguments |
| `$?` | Exit status of the last command |

<i> Note: <br>
the result of `$?`: <br>
0     → success <br>
≠ 0   → error
</i>

---

### Reading User Input

<strong> `read` Command </strong>

Used to get input from the user.

Syntax:

```bash
read variable
```

---

<strong> Prompting the User </strong>

Use `-p` to display a message:

```bash
read -p "Enter your name: " name
```

---

<strong> Reading Multiple Values </strong>

```bash
read adj1 noun1 verb1
```

Input:

```text
hairy football danced
```

Result:

```text
adj1  = hairy
noun1 = football
verb1 = danced
```

---

<strong> Example Script </strong>

```bash
#!/bin/bash

read -p "Type adjective noun verb: " adj1 noun1 verb1

echo "He sighed and $verb1 to the elixir."
echo "Then he ate the $adj1 $noun1."
```

Input:

```text
hairy football danced
```

Output:

```text
He sighed and danced to the elixir.
Then he ate the hairy football.
```

### Parameter Expansion in Bash

<strong> Parameter expansion allows you to modify or extract parts of variables. </strong>

---

Basic Syntax

These are equivalent:

```bash
$CITY
```

```bash
${CITY}
```

Curly braces are useful when the variable is next to other text:

```bash
echo "${CITY}_backup"
```

---

<strong> Default Value </strong>

- `${var:-value}`: If a variable is empty or not set, use a default value.

Example:

```bash
THIS="Example"

THIS=${THIS:-"Not Set"}
THAT=${THAT:-"Not Set"}
```

Results:

```text
THIS = Example
THAT = Not Set
```

---

<strong> Removing Text from the Beginning </strong>

- `${var#pattern}`: Remove the shortest match from the front.
- `${var##pattern}`: Remove the longest match from the front.

Example:

```bash
MYFILENAME=/home/digby/myfile.txt

FILE=${MYFILENAME##*/}
```

Result:

```text
myfile.txt
```

---

<strong> Removing Text from the End </strong>

- `${var%pattern}`: Remove the shortest match from the end.
- `${var%%pattern}`: Remove the longest match from the end.

Example:

```bash
DIR=${MYFILENAME%/*}
```

Result:

```text
/home/digby
```

---

<strong> Practical Example </strong>

```bash
MYFILENAME=/home/digby/myfile.txt

FILE=${MYFILENAME##*/}
DIR=${MYFILENAME%/*}
NAME=${FILE%.*}
EXTENSION=${FILE##*.}
```

Results:

| Variable | Value |
|-----------|--------|
| MYFILENAME | /home/digby/myfile.txt |
| FILE | myfile.txt |
| DIR | /home/digby |
| NAME | myfile |
| EXTENSION | txt |

---

<strong> Most Useful Forms </strong>
 
- `${var:-value}`: Default value if variable is empty.
- `${FILE##*/}`: Get filename from path.
- `${PATH%/*}`: Get directory from path.
- `${FILE%.*}`: Get filename without extension.
- `${FILE##*.}`: Get extension.

## Performing arithmetic in shell scripts

<strong> Bash variables are untyped, but Bash can automatically treat values as numbers when performing arithmetic. </strong>

---

### Arithmetic with `let`

Syntax:

```bash
let RESULT=$BIGNUM/16
```

Result:

```text
RESULT = 64
```
 
<i> Note: No spaces around operators. Correct: `let RESULT=$BIGNUM/16`, Wrong: `let RESULT = $BIGNUM / 16`.</i>

---

### Arithmetic with `expr`

Syntax:

```bash
RESULT=`expr $BIGNUM / 16`
```

Result:

```text
64
```

<i> Note: Spaces are required. Correct: `expr 1024 / 16`, Wrong: `expr 1024/16`.</i>

---

### Arithmetic with `bc`

Syntax:

```bash
RESULT=`echo "$BIGNUM / 16" | bc`
```

Result:

```text
64
```

Features:

- Supports floating-point numbers
- Flexible spacing

---

### Random Numbers

```bash
let foo=$RANDOM
echo $foo
```

`$RANDOM` generates a random integer.

---

### Incrementing Variables

<strong> Pre-increment: Increase first, then use value </strong>

```bash
I=1
echo $((++I))
```

Output:

```text
2
```

---

<strong> Post-increment: Use current value, then increase </strong>

```bash
I=1
echo $((I++))
echo $I
```

Output:

```text
1
2
```

---

## Programming contructs

### Bash `if` Statements

Used for conditional execution.

---

#### if...else

```bash
if [ condition ]; then
    commands
else
    commands
fi
```

Example:

```bash
STRING="Friday"

if [ "$STRING" = "Friday" ]; then
    echo "WhooHoo. Friday."
else
    echo "Will Friday ever get here?"
fi
```

---

#### if...elif...else

```bash
if [ condition1 ]; then
    commands
elif [ condition2 ]; then
    commands
else
    commands
fi
```

Example:

```bash
filename="$HOME"

if [ -f "$filename" ]; then
    echo "Regular file"
elif [ -d "$filename" ]; then
    echo "Directory"
else
    echo "Unknown"
fi
```

---

### Bash Test Operators

#### File Tests

| Operator | Meaning | Memory Trick |
|----------|---------|-------------|
| `-e file` | File exists | **e = exists** |
| `-f file` | Regular file | **f = file** |
| `-d file` | Directory | **d = directory** |
| `-r file` | Readable | **r = read** |
| `-w file` | Writable | **w = write** |
| `-x file` | Executable | **x = execute** |
| `-s file` | Exists and size > 0 | **s = size** |
| `-L file` | Symbolic link | **L = Link** |
| `-h file` | Symbolic link | Same as `-L` |
| `-O file` | You own the file | **O = Owner** |

---

#### String Tests

| Operator | Meaning | Memory Trick |
|----------|---------|-------------|
| `-n string` | String is not empty | **n = not empty** |
| `-z string` | String is empty | **z = zero length** |
| `str1 = str2` | Strings are equal | Standard equality |
| `str1 != str2` | Strings are not equal | Standard inequality |

---

#### Numeric Comparisons

| Operator | Meaning | Memory Trick |
|----------|---------|-------------|
| `-eq` | Equal | **eq = equal** |
| `-ne` | Not equal | **ne = not equal** |
| `-gt` | Greater than | **gt = greater than** |
| `-lt` | Less than | **lt = less than** |
| `-ge` | Greater or equal | **ge = greater/equal** |
| `-le` | Less or equal | **le = less/equal** |

---

#### Logical Operators

| Operator | Meaning | Memory Trick |
|----------|---------|-------------|
| `expr1 -a expr2` | AND | **a = and** |
| `expr1 -o expr2` | OR | **o = or** |
| `! expr` | NOT | Negation |

---

#### File Time Comparisons

| Operator | Meaning | Memory Trick |
|----------|---------|-------------|
| `file1 -nt file2` | file1 is newer | **nt = newer than** |
| `file1 -ot file2` | file1 is older | **ot = older than** |
| `file1 -ef file2` | Same file/link | **ef = equal file** |

---

#### Rarely Used (System Administration)

| Operator | Meaning |
|----------|---------|
| `-b file` | Block device |
| `-c file` | Character device |
| `-g file` | SGID bit set |
| `-k file` | Sticky bit set |
| `-p file` | Named pipe |
| `-S file` | Socket |
| `-t fd` | Terminal descriptor |
| `-u file` | SUID bit set |

---

### Bash Shortcuts for `if`

Instead of writing a full `if...then`, Bash allows short one-line conditions.

---

#### Execute if FALSE (`||`)

Syntax:

```bash
[test] || command
```

Meaning: If the test is **false**, execute the command.

Example:

```bash
dirname="/tmp/testdir"

[ -d "$dirname" ] || mkdir "$dirname"
```

---

#### Execute if TRUE (`&&`)

Syntax:

```bash
[test] && command
```

Meaning: If the test is **true**, execute the command.

Example:

```bash
[ $# -ge 3 ] && echo "There are at least 3 arguments."
```

---

#### One-line if...else (`&&` + `||`)

Syntax:

```bash
[test] && command1 || command2
```

Meaning:

```text
If test is TRUE  → run command1
Else             → run command2
```

Example:

```bash
dirname="mydirectory"

[ -e "$dirname" ] && echo "$dirname already exists" || mkdir "$dirname"
```

---

### `case` command

`case` is used when you need to check one value against multiple possible options.

It is often cleaner and easier to read than many nested `if...elif...else` statements.

---

<strong> General Syntax </strong>

```bash
case VARIABLE in
    value1)
        commands
        ;;
    value2)
        commands
        ;;
    *)
        default commands
        ;;
esac
```

<strong> Important Parts </strong>

| Syntax | Meaning |
|----------|----------|
| `case` | Start of case statement |
| `value)` | Condition to match |
| `;;` | End of a case branch |
| `|` | OR |
| `*` | Default / catch-all case |
| `esac` | End of case (`case` backwards) |

---

<i> Example </i>

```bash
read -p "Choose a color: " color
case "$color" in
    red)
        echo "You chose red."
        ;;
    blue)
        echo "You chose blue."
        ;;
    green)
        echo "You chose green."
        ;;
    *)
        echo "Unknown color."
        ;;
esac
```

---

<i> Using OR (`|`) </i>

```bash
case "$answer" in
    y|Y)
        echo "Yes"
        ;;
    n|N)
        echo "No"
        ;;
    *)
        echo "Invalid input"
        ;;
esac
```

---

### `for...do` Loop

`for` loop is used to repeat actions for every item in a list.

---

#### General Syntax 

```bash
for VARIABLE in LIST
do
    commands
done
```

Or in one line:

```bash
for VARIABLE in LIST ; do
    commands
done
```

---

<i> Example </i>

```bash
for NAME in John Paul George Ringo
do
    echo "$NAME"
done
```

Output:

```text
John
Paul
George
Ringo
```

---

<i> Example with Files </i>

```bash
for FILE in *
do
    echo "$FILE"
done
```

`*` means all files in the current directory.

---

#### C-style for Loop

Structure:

```bash
for ((initialization; condition; increment))
```

Example:

```bash
for ((i=1; i<=10; i++))
do
    echo "$i"
done
```

---

### `while...do` and `until...do` Loops

Used when repetition depends on a condition.

---

#### `while` Loop

<strong> General Syntax </strong>

```bash
while [ condition ]
do
    commands
done
```

---

<i> Example </i>

```bash
N=0

while [ $N -lt 5 ]
do
    echo $N
    let N=$N+1
done
```

Output:

```text
0
1
2
3
4
```

---

#### `until` Loop

<strong> General Syntax </strong>

```bash
until [ condition ]
do
    commands
done
```

---

<i> Example </i>

```bash
N=0

until [ $N -eq 5 ]
do
    echo $N
    let N=$N+1
done
```

Output:

```text
0
1
2
3
4
```

---

### Comparison

| Loop | Runs When |
|--------|------------|
| `while` | Condition is TRUE |
| `until` | Condition is FALSE |

---

<i> Same Logic Example </i>


```bash
while [ $N -lt 10 ]

until [ $N -eq 10 ]
```

Meaning:

```text
Run while N < 10
```

---