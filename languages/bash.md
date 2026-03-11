
# sh & shell

- [ ] sh vs bash, history
- [ ] ***bourne again shell?***

- `sh` Bourne shell, the standard command language interpreter.
	- will start an interactive shell session
	- `-c "command"` run a command and then quit
	- `sh path/script.sh` execute a script
		- TODO: but we can simply do path/script.sh what is the difference
	- `-s` read and execute commands from stdin 
# creating & running a script

## file creation & running

- create a script at
	- `~/bin/helloworld.sh` (common location for personal scripts)
	- or `~/.local/bin`
- make it executable with: 
	- `chmod +x ~/bin/helloworld.sh`
- ausführen:
	- `./bin/helloworld argument1`
- add it to your aliases in `~/.zshrc`

# basics of scripting
## shebang

> 🥏 TODO. There is much more to learn here.

> `#!/usr/bin/env bash`
> It is the first line in any bash script (to my understanding)
> It tells the operating system **which interpreter should run the script**.

- `#!` → Marks that this line specifies an interpreter
- `/usr/bin/env bash` → Run the script using the `bash` interpreter found by `env`
	- This is better than specifying directly `#!/bin/bash` because bash may not be there on every os, and the preferred way as above searches **PATH** for the first bash executable it finds there and uses it
	- `/usr/bin/env` is an executable
	- to see what it would return do `which bash`

## "strict mode"

> `set -euo pipefail`

- `-e` 
	- exit immediately if any command returns a non-zero status
	- prevents your script from quietly continuing after an error
- `-u`
	- treat unset variables as an error and exit
	- helps you catch typos and missing variables
- `-o pipefail`
	- make pipelines fail if _any_ command fails
	- any error in the pipeline causes the whole pipeline to fail
	- otherwise a derp like `false | grep something` would be successful, because by default bash cares only about the *exit command* of the last command in the pipe
	- in connection with `-e` this will stop the script at the point of failure

## Exit-Codes

- ***0*** - success
- ***1-255*** - failure

### error handling

```bash
command || {
  # this will be called only if command returned non-0 status code
}
```

## arithmetic

> Arithmetic syntax (( )), ***think of `(( ))` as a math-only zone***.
```bash
# Basic examples:
a=5
b=3

# Expansion contexts (need $)
# notice you dont need $ before a and b variables
# you need the $ before the get the result
result=$(( a + b )) # Assign to variable
echo $(( a * b )) # Pass as argument
array[$(( i+1 ))]="value" # Use as index



# NOTICE $ BEFORE THE (( ))
sum=$(( ${numbers_array[1]} + 5 ))

# Evaluation contexts (no $)
(( a > b )) && echo "yes"  # Condition in if/&&
for (( i=0; i<10; i++ )); do  # For loop arithmetic

# DON'T USE $ BEFORE WHEN DOING ARITHMETIC EVALUATION
# see more in notes on if
# Arithmetic EVALUATION - tests a CONDITION
if (( ${fruits[1]} > 10 )); then
    echo "Greater"
fi
```

## loops

### for-loop

```bash
for i in $(seq 1 10); do
	echo "$i"
done
```

### while-loop

```bash
while BEDINGUNG; do
  # Befehle
done
```

#### bonus IFS & <<<

> ***Internal Field Separator***
> 
> It is probably not a perfect place for it, but it seems to be actually very commonly used with while loops.

> basics
```bash
# By default IFS is set to: whitespace, tab and newline

# IFS=':'  - An Doppelpunkten trennen
pfade="/usr/bin:/bin:/usr/local/bin"
IFS=':'
for pfad in $pfade; do
    echo "📁 $pfad"
done

# Ohne IFS-Änderung (Standard)
text="Hallo   Welt   Bash"
for word in $text; do
    echo "'$word'"
done
# Ausgabe:
# 'Hallo'
# 'Welt'
# 'Bash'
# (Mehrere Leerzeichen werden ignoriert!)

# Mit IFS= (nichts trennen)
text="Hallo   Welt   Bash"
for IFS= word in $text; do
    echo "'$word'"
done
# Ausgabe: 'Hallo   Welt   Bash' (alles ein Wort)
```

> more advanced example
```bash
# Consider a situation
readonly CONSTANTS=$(python3 -c "
from src.constants import (
    VERSION,
    BBC_SOUNDS_CACHE_DIR,
)
print(f'BBC_SOUNDS_CACHE_DIR={BBC_SOUNDS_CACHE_DIR}')
print(f'VERSION={VERSION}')
")

while IFS= read -r line; do
    export "$line"
    echo "with ifs: " "$line"
done <<< "$CONSTANTS"

# notice you could also write:
IFS=
while read -r line; do
	...
# but then you would set IFS globally and obviously it is much safer to set it just for the loop you are working on
```
## quotes ' or "

> single quotes, *NO EXPANSION*
```bash
name="Jim"
echo 'This is $name, a developer'
# prints exactly: This is $name, a developer
# does NOT expand $name

# BUT
echo 'This is '$name', a developer'
# will print: This is Jim, a developer
```

> double quotes, *EXPAND*
```bash
name="Jim"
echo "This is $name, a developer"
# prints what you would expect: This is Jim, a developer

# escaping characters with \
# like when you want to use some special sign literally
# example where you want to print $ sign
money=100
echo "You have ${money}\$"
```
# building blocks

## die Variable

#### declaration
```bash
# declare a variable
NAME="Alinka"

"NOTICE SPACE AROUND '=' IS FORBIDDEN"

# declare a constant
readonly MAX_USERS=100

# declaring variables within functions
# they require `local`
# and commonly use lower-case naming
local my_text="meow"
```

#### referencing
```bash
# THE BULLETPROOF VARIABLE REFERENCE PATTERN:
# ALWAYS SURROUND THEM WITH "" AND {}
# this is not always necessary, but as a beginner you will be safe this way
# The {} just makes it explicit where variable name ends
ls "${DIR}"

# EXAMPLE WHAT CAN GO OTHERWISE WRONG:
# Without {} - ambiguous
FILE="document"

# Bash looks for variable "FILE.txt" which doesn't exist!
echo "$FILE.txt"    # Prints: (nothing).txt

# With {} - clear
# Bash knows "FILE" is the variable name, ".txt" is literal
echo "${FILE}.txt"  # Prints: document.txt

# 1. Bash sees the line: meow 14 "$oik"
# 2. It expands $oik: meow 14 "true oik"
# 3. Then it executes: meow with arguments "14" and "true oik"
command="meow 14 \"$oik\""
```

#### predefined variables

```bash
#!/bin/bash
echo "Skript-Name: ${BASH_SOURCE[0]}"
echo "Aktuelle Funktion: ${FUNCNAME[0]}"
echo "Zeilen-Nummer: ${LINENO}"
echo "Bash-Version: ${BASH_VERSION}"
echo "Hostname: ${HOSTNAME}"
echo "Prozess-ID: $$"

- `$0` der Name des Scripts
- `$1` erste argument
- `$2` zweite argument
- `$#` Anzahl der Argumente
echo "Status code of last c"
```

##### BASH_SOURCE
```bash
# Find the Project Root, where the project is.
SCRIPT_DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# Useful when working with Python:
source "$PROJECT_ROOT/.venv/bin/activate"
export PYTHONPATH="${PYTHONPATH:-}:$PROJECT_ROOT"
```
#### variable expansion

```bash
# 1. Basic variable (braces optional for simple cases)
name="Merryangel"
echo $name      # Simple, but ambiguous sometimes
echo ${name}    # Explicit, always safe

# 2. Disambiguate from surrounding text
echo ${name}_is_awesome    # With braces: "Merryangel_is_awesome"

# 3. Default values
echo ${username:-guest}    # Use "guest" if username unset
echo ${filename:?Error: file missing}  # Error if unset

# 4. String manipulation
echo ${name:0:4}           # Substring: "Merr"
echo ${name/M/m}            # Replace: "merryangel"
echo ${#name}               # Length: 10

# 5. Pattern removal
filename="archive.tar.gz"
echo ${filename%.*}         # Remove shortest suffix: "archive.tar"
echo ${filename%%.*}         # Remove longest suffix: "archive"
echo ${filename#*.}          # Remove shortest prefix: "tar.gz"

# 6. Indirect expansion (what we saw earlier)
# INTERESTING
# you can use string contents of a variable to use it as an sctual variable name that you want to call
var_name="name"
echo ${!var_name} # Expands to value of $name: "Merryangel"
```

#### ${!var_name}

> This is called `indirect expansion`.
> 
> It makes a copy of variable using the name of variable after ! sign.
> 
> It was also used to pass variables by reference, but this was replaced by

```bash

# Setup
fruit="apple"
color="red"
item="fruit"

# Direct expansion
echo $fruit # "apple"

# Indirect expansion
echo ${!item} # "${!fruit}" = value of variable 'fruit' = "apple"

# Another example
thing="color"
echo ${!thing} # "${!color}" = value of variable 'color' = "red"

# Indirect expansion (works for any variable)
# Makes a COPY of the array passed by name
local list_data=("${!1}")  # For arrays
local scalar_data="${!1}"   # For scalar variables

# Example with scalar
name="Merryangel"

get_value() {
    local var_name="$1"
    local value="${!var_name}"  # Indirect expansion
    echo "$value"
}

get_value "name"  # Prints: Merryangel
```

#### export

- Valuable notes: https://www.baeldung.com/linux/bash-variables-export
- You can export a variable to make it an `ENVIRONMENT VARIABLE` 
	- so that it is available in subprocesses of your script

```bash
export AUTO_MODE_FILE="/tmp/tsb_auto_mode_$$" # $$ is process id
echo "false" > "$AUTO_MODE_FILE"
```

#### UNTYPED or are bash variables all arrays? 

> https://tldp.org/LDP/abs/html/untyped.html#BVUNTYPED
> 
> Bash permits array operations on variables, even if the variables are not explicitly declared as arrays. 
```bash
string=abcABC123ABCabc  
echo ${string[@]} # abcABC123ABCabc  
echo ${string[*]} # abcABC123ABCabc  
echo ${string[0]} # abcABC123ABCabc  
echo ${string[1]} # No output!  
# Why?  
echo ${#string[@]} # 1  
# One element in the array.  
# The string itself.  
  
# Thank you, Michael Zick, for pointing this out.  
  
# Once again this demonstrates that Bash variables are untyped.
```
## function

```bash
# definition
meow() {
	"$1 $2 $3 etc. to use values passed in"
	"***notice they override global script parameters***"
}

# old definition
function meine_function() {
	# meow!
}

VAR="apple pie"

# calling a function
# MORE PROBLEMATIC THAN YOU WOULD HAVE THOUGH
# 1. IMPORTANT:
# "${variablename}" is the only fully brainsafe way to pass a variable into the function
# 2. BUT!!!
# FOR ARRAYS YOU WILL PASS IN JUST THE ARRAY NAME, SEE BELOW IN READING ARGS
# 3. NOTICE!! derp
# this will basically pass in a string "derp" 
meow 14 "oik" "${VAR}" derp

# calling function while capturing its result
result=$(meow 14 "oik")
# IMPORTANT, this pattern is also used for calling other bash commands direct from your script, the resolution order goes:
# 1. A function defined in the current script
# 2. A built-in command
# 3. An alias
# 4. An executable in PATH
# 5. A file with path (./something, /bin/something)
```


### reading args

```bash
# Reading basic args
my_function() {
    local first="$1"      # First argument
    local second="$2"     # Second argument
    local all=("$@")      # All arguments as array
    local count="$#"      # Number of arguments
    
	local -n ref="$3"     # Reference (can modify original)
	ref="tomato"
}

# Call it
fav_fruit="cherry"
my_function "apple" "banana" "fav_fruit"
echo $fav_fruit # will log "tomato"!!!

# ARRAYS ARE PROBLEMATIC
fruits=("apple" "banana" "cherry")
declare -A user=([name]="Merryangel" [shell]="bash")

# ❌ This FAILS miserably
# Only passes "apple"
func "$fruits" 

# ❌ This also FAILS
# Passes as separate args, loses array structure
func "${fruits[@]}" 

# ✅ This WORKS - pass by name
func "fruits" "user"

# Inside function:
func() {
	# Reconstruct a work copy array
	# DON'T DO IT, USE BY REF BELOW
    local -a local_fruits=("${!1}")
    
    # BASICALLY ALWAYS USE BY REF:
    # if you try to copy you will have problems when the array contains strings with whitespaces,
    # so always do this
    local -n ref="$1"
    # if you really need a copy and don't want changes in the original, then inside safely copy the array with:
    local -a copy=("${ref[@]}")
    
    # FOR DICTIONARY U DON'T EVEN HAVE A CHOICE
    local -n ref="$2"
}
```

### nameref (local -n ref=$"1")

> Passing arguments by reference
> 
> For the old method to do it, see [[#indirect expansion]].
```bash
# NEW METHOD: Nameref (bash 4.3+) - works for ANY variable type
modify_var() {
	# ref is a reference to the variable named in $1
    local -n ref="$1"
    # This modifies the original!
    ref="new value"    
}
myvar="old value"
modify_var "myvar"

# Prints: "new value" (was modified!)
echo "$myvar" 

# Works with arrays too!
modify_array() {
    local -n arr_ref="$1"
    arr_ref+=("new element")  # Adds to original array
}

myarray=("apple" "banana")
modify_array "myarray"

# Prints: apple banana new element
echo "${myarray[@]}"  
```

### returning things

> Two easiest way to capture results of a method:
```bash
#!/bin/bash

# METHOD 1: simply echo stuff
get_fruits() {
    echo "apple"
    echo "banana"
    echo "cherry"
}

# Capture as multi-line string
result=$(get_fruits)
echo "$result"
# Output:
# apple
# banana
# cherry

# METHOD 1.1: Kindof the same but somehow neater
# Or read into array
mapfile -t fruits < <(get_fruits)
echo "${fruits[1]}"  # banana

# METHOD 2: With nameref array, 
results=()
get_stuff {
	local -n arr_ref="$1"
	results=("oik" "meow")
	results+=("hrrr")
}
get_stuff "results"
echo "${results[@]}"
```

## command substitution

```bash
# runs command, returns output
# see how this structure is resolved in notes on a function
result=$(command)

# example:
files=$(ls)

# BUT!!!!

# Pattern 1: One-liner (if you don't need the array later)
result=$(meow 14 "$oik")

# Pattern 2: With array (if you need to reuse/modify the command)
cmd=(meow 14 "$oik")
result=$("${cmd[@]}")

# !!!
# Pattern 3: With additional arguments later
cmd=(meow 14)
result=$("${cmd[@]}" "$oik")  # Append extra arg
```
### calling python scripts

`basic calling with arguments`
```python
#!/usr/bin/env python3
import sys
from mymodule import func1, func2, func3

if __name__ == "__main__":

    if len(sys.argv) < 2:
        print("Usage: ./run.py <function> [args...]")
        sys.exit(1)
    
    func_name = sys.argv[1]
    args = sys.argv[2:]
    
    if func_name == "func1":
        result = func1(*args)
    elif func_name == "func2":
        result = func2(*args)
    elif func_name == "func3":
        result = func3(*args)
    else:
        print(f"Unknown function: {func_name}")
        
        # this lets you return an error code 1 to bash
        sys.exit(1)
        # TODO how to handle that code on bash side 
    
    print(result)
```
```bash
#
./run.py func2 42

# interestingly anything you print() in python you will get this captured as stdout
# so you basically print stuff in python as a way of returning things
COUNT=$(python3 ./run.py 42)
```

## array

### arrays are string arrays

> 🔥 ***All bash arrays are string arrays***, everything is stored as a string.
```bash
fruits=("apple" 42 3.14 "cherry" 7.0)
# All elements are strings: "apple", "42", "3.14", "cherry", "7.0"

echo "${fruits[1]}"     # Prints: 42 (but it's a string!)

# You can do math, but bash converts
sum=$(( ${fruits[1]} + 5 ))  # 42 + 5 = 47 (bash converts strings to numbers)
```

### indexed arrays

```bash
# Indexed Arrays
# Initialized with values
fruits=("apple" "banana" "cherry")

# Multi-line
commands=(
    "ls -la"
    "ps aux"
    "df -h"
)

# you knidof dont need to use "" when there are no whitespaces
# BUT AVOID THIS, LOOK BELOW, CAT WILL GET YOU IN TROUBLE
weird=(
	meow
	cat # TROUBLE, THIS IS A COMMAND
	# YOU WOULDN'T HAVE TROUBLE IF YOU USED ""
	drinks
	milk
)

# Or create empty and add items
fruits=()
fruits[0]="apple"
fruits[1]="banana"
fruits[2]="cherry"

# a
fruits[100]="mystery pe"

# Add elements
fruits+=("lemon")
fruits+=(lemon) # THE SAME FOR STRINGS WITH NO WHITESPACES

# CRUCIAL
# Add at specific index
# NOTICE A WILD THING
# Bash arrays can have "holes" (sparse arrays).
fruits[100]="mystery pie"
# then when you do index expansion:
for i in "${!fruits[@]}"; do
	echo "$i"
done
# you will suddenly have an iindex jump to 100!

# Add multiple at once
fruits+=("berries" "tomato")

# As a result:
for i in "${!fruits[@]}"; do
	echo "$i: Fruit: ${fruits[$i]}"
done
# will return
# NOTICE NO 45H INDEX
0: Fruit: apple
1: Fruit: banana
2: Fruit: cherry
3: Fruit: lemon
5: Fruit: orange
6: Fruit: berries
7: Fruit: tomato

# Remove by index
unset 'fruits[1]'  # Removes "banana"
echo "${fruits[@]}"  # Prints: apple cherry date

# Remove entire array
unset fruits
```

### accessing array elements

```bash
fruits=("apple pie" "banana split" "cherry")

# common mistake
# this prints just the first element
echo "$fruits"

# Get single element (index starts at 0)
echo "${fruits[1]}"  # Prints: banana
echo ${fruits[-1]}  # cherry (last element)
echo ${fruits[-2]}  # banana split (second to last)

# Get all elements
# EXPAND ALL ARRAY ELEMENTS ES SEPARATE ARGUMENTS
# Prints: apple pie banana split cherry
echo "${fruits[@]}"

# STRING DETOUR:
# then an example:
string_data=$(printf '%s\n' "${fruits[@]}")
# printf receives: 
# "apple pie" "banana split" "cherry" as THREE arguments
# Format '%s\n' is applied to each: 
# "apple pie\n", "banana split\n", "cherry\n"
# Notice this thing is no longer an array!
# after this:
echo "${#string_data}"    # Length in characters (maybe 14)
echo "${string_data[1]}"  # Second CHARACTER, not second element!

echo "${fruits[*]}"  # Also prints all, but treated as one string

# Get array length
echo "${#fruits[@]}"  # Prints: 3

# Get indices
echo "${!fruits[@]}"  # Prints: 0 1 2
# also:
# ${!fruits[@]} expands to an array of indices
for i in "${!fruits[@]}"; do
    echo "Index $i = ${fruits[$i]}"
done

# IMPORTANT, CRITICAL DIFF. BETWEEN @ AND *

# [@] - Each element as separate item (preserves spaces)
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done
# Output:
# Fruit: apple pie    ← "apple pie" stays together!
# Fruit: banana split ← "banana split" stays together!
# Fruit: cherry

# [*] - All elements as one string
for fruit in "${fruits[*]}"; do
    echo "Fruit: $fruit"
done
# Output:
# Fruit: apple pie banana split cherry  ← All in one line!
```

### array slicing

```bash
numbers=(0 1 2 3 4 5 6 7 8 9)

# Result: (1 2 3 4)
# note, you need the surrounding () because we want the slice to be an array aswell, this is an array assignment
slice=("${numbers[@]:1:4}")
```

### array pitfalls

```bash
# This creates TWO separate arguments to the command
# Two args: "-i" and "/path/to/key"
ssh "-i" "${config[key]}"  
# This creates ONE argument with spaces inside
# One arg: "-i /path/to/key" (WRONG for ssh!)
ssh "-i ${config[key]}"  

# creating a list of arguments for a command
# if you would put "-i" "${config[key]}"
# in a single string ssh would be unhappy
# since it would interpret it as a single string
# where it needs two arguments -i and following key
ssh_cmd=(
    "ssh"
    "-i" "${config[key]}"
    "${config[user]}@${config[host]}"
    "-p" "${config[port]}"
)
"${ssh_cmd[@]}"
```

### associative arrays

```bash
# Declare first (bash 4+)
TODO: what is this declare and what are the other uses
declare -A user
user["name"]="Merryangel"
user[name]="Merryangel" # SAME THING!
user["full name"]="M Merryangel"  # Quotes needed because of space
user["shell"]="bash"
user["server"]="doomhamster"
```

> Example: building `fzf` arguments:
```bash
build_fzf_args() {
    local multi="$1"
    local preview="$2"
    
    # Start with base arguments
    local args=(
        --style full
    )
    
    # Conditionally add options
    if [[ "$multi" == "true" ]]; then
        args+=(--multi)
    fi
    
    if [[ -n "$preview" ]]; then
        args+=(--preview="$preview")
    fi
    
    # Use the array
    fzf "${args[@]}"
}
```

# comparison

## if/fi

```bash
"NO BRACKETS WHEN JUST CHECKING ERROR CODES, LIKE"
if command -v fzf &> /dev/null; then
	echo "fzf not found, it is required"
	return 1
fi

#another example:
if kill -0 "$LAST_MPV_PID" 2>/dev/null; then
  # Wird NUR ausgeführt, wenn kill -0 erfolgreich ist (Exit-Code 0)
  # Also wenn der Prozess EXISTIERT
else
  # Wird ausgeführt, wenn kill -0 fehlschlägt (Exit-Code != 0)
  # Also wenn Prozess NICHT existiert
fi

"[[ ]] FOR STRING COMPARISON, FILE CHECKS, REGEX"
# you can it use for numbers but then you need those clunky flags
if [[ "$CAT_NAME" == "Alinka" ]]; then
	# stuff
elif [[ ANDERE_BEDINGUNG ]]; then
	# more stuff
else
	# meow
fi

"string checks"
[[ -z "$a" ]]            # Ist leer
[[ -n "$a" ]]            # Ist nicht leer

"file checks"
[[ -f "$file" ]]         # Ist eine Datei?
[[ -d "$dir" ]]          # Ist ein Verzeichnis?
[[ -e "$path" ]]         # Existiert?
[[ -r "$file" ]]         # Lesbar?
[[ -w "$file" ]]         # Schreibbar?
[[ -x "$file" ]]         # Ausführbar?

"multiple conditions"
if [[ -f "$filepath" ]] && [[ ! -f "$temppath" ]]; then

"regex"
# DONT USE "" BRACKETS FOR REGEX SPELLS
# match: =~
[[ $TEXT =~ ^[0-9]+$ ]]

"(( )) FOR NUMBER COMPARISON"
# == and !=
# > and >=
# < and <=
if (( $USER_COUNT > 5 )); then
	...
fi

"if you must use number comparison in [[ ]], then u must use flags:"
| -eq      | equal        |
| -ne      | not equal    |
| -gt      | greater than |
| -lt      | less than    |
ie.
[[ $COUNT -eq 5 ]]

"( ) OLD SINGLE BRACKETS"
# you might encounter them
# don't use them
```
## case

```bash
case "$1" in
	"start"|"run")
		# stuff
		;;
	"stop")
		# stuff
		;;
	*)
		echo "Ich kenne '$1' nicht."
		exit 1
		;;
esac
```

# shell built-in commands

### let

> Evaluate arithmetic expressions in shell. 
> More information: https://www.gnu.org/software/bash/manual/bash.html#index-let.
```bash
- Evaluate a simple arithmetic expression:  
   let "result = a + b"  
  
 - Use post-increment and assignment in an expression:  
   let "x++"  
  
 - Use conditional operator in an expression:  
   let "result = (x > 10) ? x : 0"
```
### printf

> convert an array to a `\n-separated` string
```bash
string_data=$(printf '%s\n' "${fruits[@]}")
```
### read

> Used to retrieve data from *stdin*.

> `read -p "Input: " VARIABLE`
> 
> commonly this might mean getting input from the user
```bash
# will wait for your input
read my_name 
echo $my_name

# will let you input studd
read -p "Enter your input here: " my_name
echo $my_name

read <<< "The surname is Bond" _ variable1 _ variable2
echo $variable1 # surname
echo $variable2 # bond
```

> `readline -e -p "Path: " DIR` 
- https://stackoverflow.com/questions/4819819/get-autocompletion-when-invoking-a-read-inside-a-bash-script
- will allow for autocompletion for path
- `-e`
	- "*If the standard input is coming from a terminal, Readline is used*
    *to obtain the line.*"

> other common flags
- `read -r variable`
	- do not let backslash (\) act as an escape character
- `read -s variable`
	- do not echo typed characters (secret ninja mode)
- `read -a array`
	- store each of the next lines you enter as values of an array

### set
### trap

> When X (signal) happens, do Y (command)

```bash
trap 'befehle' SIGNAL
#    ↑          ↑
#    Was        Wann
#    tun        passiert's

# example, remove temp dir on EXIT signal
temp_dir=$(mktemp -d)
trap 'rm -rf "$temp_dir"; echo "🧹 Aufgeräumt!"' EXIT


# other signals
# lets assume you have a 'cleanup' function
trap cleanup EXIT      # Beim normalen Ende
trap cleanup INT       # Bei Ctrl+C
trap cleanup TERM      # Bei kill
trap cleanup ERR       # Bei Fehler (set -e)

# Kombiniert:
trap cleanup EXIT INT TERM  # Mehrere Signale
```

# sugar

## & im Hintergrund $!

> Adding `&` at the end of a command to make it execute in the backgound without blocking.

> & as nonblocking
```bash
# Ohne & (blockierend)
sleep 5
echo "fertig"  # Wird erst NACH 5 Sekunden ausgeführt

# Mit & (nicht-blockierend)
sleep 5 &
echo "sofort"  # Wird SOFORT ausgeführt, sleep läuft parallel
```

> ***IMPORTANT***!
> 
> This is how you can get the PID of the most recently executed backgorund pipeline, like in case you want to kill it or what.
> 
> https://unix.stackexchange.com/questions/85021/in-bash-scripting-whats-the-meaning-of
> 
> `! - Expands to the process ID of the most recently executed background (asynchronous) command.`
> 
> And then you access that as usually with `$`.
```bash
mpv --no-video --no-terminal "${filepath}.mp3" &
MY_PID=$!
```
## ANSI colourzzz

> https://gist.github.com/JBlond/2fea43a3049b38287e5e9cefc87b2124

> basic structure
```bash
\e[     STYLE;FOREGROUND;BACKGROUND   m
 ↑      ↑                             ↑
Escape  Start                         Ende
Code
```

> style codes
```bash
0  # Alles normal
1  # Fett
2  # Dunkler (selten)
3  # Kursiv
4  # Unterstrichen
5  # Blinkend (langsam)
6  # Blinkend (schnell)
7  # Invertiert
8  # Versteckt
9  # Durchgestrichen
```

> colourz
> the helles / high intensity are generally prettier
```bash
30  # Schwarz       90  # Helles Schwarz (Grau)
31  # Rot           91  # Helles Rot
32  # Grün          92  # Helles Grün
33  # Gelb          93  # Helles Gelb
34  # Blau          94  # Helles Blau
35  # Lila          95  # Helles Lila
36  # Cyan          96  # Helles Cyan
37  # Weiß          97  # Helles Weiß
```

> examples
- `echo "default meow \e[0;97mpretty white meow \e[1;37mBOLD MEOW"`
- `echo "\e[5m\e[1m\e[3;32mBLINKY MEOW"`
	- `\e[5m` blink
	- `\e[1m` bold
	- `\e[3;32m` italic green
# Argumente / Parameters

## basic parameters


## parameter expansion

### basic form

> 

**${var:-default}**
- if **var** has value, use it
- if it douesn't, use default
- *DON'T change var-*

**${var:=default}**
- as above but it *will change var*

**${var:?message}**
- if **var** is empty, script stops with an error
- you specify the error **message**
- example: `FILE=${1:?You must pass a file name}`

**${var:+alternate}**
- i not understand
- not often used

`DIR="${1:-.}"`
- `${1}` means “the first argument passed to the script”
- `:-` means “use a default value if empty or unset”
- `.` means “current directory”

It is equivalent to:
```bash
if [ -n "$1" ]; then
    DIR="$1"
else
    DIR="."
fi
```

# string operations

> TODO OKI, there are many ways to operate on the strings in

**upper / lower case**
```bash
${var^^}   # all uppercase
${var,,}   # all lowercase
${var^}    # capitalize first letter
```

# operators

### separator, AND, OR

- `cmd1; cmd2`
	- will run cmd2 regardless if cmd1 fails
- `cmd1 && cmd2`
	- will run cmd2 only if cmd1 was successful
	- `aka it returns code 0`
- `cmd1 || cmd2`
	- will run cmd2 only `if cmd1 fails`
- examples:
	- `make && echo "Build succeeded" || echo "Build failed"`
		- if make failed, the echo after && will be skipped and will go directly to echo after ||
	- `make && { echo "Build succeeded"; do_other_things; } || echo "Build failed"`
		- grouping with `{}` ensures the `||` only reacts to `make`, not to later commands.