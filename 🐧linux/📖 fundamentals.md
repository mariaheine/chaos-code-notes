****## todo
- [ ] understand how the whole process behind adding a package like in here https://apt.syncthing.net/
- [ ] https://refspecs.linuxfoundation.org
- [ ] https://labex.io/linuxjourney
- [ ] lsblk -f
- [ ] df -h
- [ ] uname
- [ ] jq (json processing)

# learning resources

- [command line fu](https://www.commandlinefu.com/commands/browse)
- 

# linux filestructure

# 🐖 basics

> Probably everything here is part of #coreutils , see full list of coreutils here: https://www.gnu.org/software/coreutils/manual/html_node/index.html

## terminal shortcuts

- `ctrl + shift + c/v` 
	- copy/paste from clipboard into 
command line
- `ctrl + c` 
	- cancel current command
- `ctrl + r` 
	- start revers search in command history, just start typing then the searched part of command
	- `repeat ctrl + r` will go with the same search pattern one step back
	- `enter` runs found command
	- `right arrow` prints that command
	- `ctrl + g` cancel
- command readline shortcuts
	- movement
		- `ctrl + a` jump to the start
		- `ctrl + e` jump to the end
		- `alt + f` move forward one word
		- `alt + b` move back one word
	- editing
		- `ctrl + k` delete from cursor too the end
		- `ctrl + w` delete word before cursor
		- `alt + d` delete word after cursor
	- you can customize those shortcuts by editing `~/.inputrc`
- `ctrl + l` clear
- `ctrl + d` close shell if line is empty

## stdin/stdout/stderr

- `>` ***stdout - 1*** 
	- output redirection operator
	- `echo Hello World > peanuts.txt` 
		- will print Hello World to peanuts.txt and overwrite the file
	- `>>` 
		- will instead append to the peanuts.txt instead of overwriting it
	- examples:
		- `> "${HOME}/.cache/myapp/logs/last.log"`
			- will empty the last.log file
- `<` ***stdin - 0*** 
	- input redirection operator
	- `cat < peanuts.txt > banana.txt` 
		- content of peanuts.txt gets redirected to both cat and banana.txt
- `2>` ***stderr - 2***
	- `ls /fake/directory 2> peanuts.txt` 
		- will print the error message into peanuts.txt
	- `2>&1` 
		- redirect stderr to stdin
	- `&>` 
		- shortcut for above
		- *but expects a path to file has following it* 
		- (can be used only in that context)
	- `ls /fake/directory &> peanuts.txt` 
		- will print both possible stdout and stderr into the peanuts.txt, but you wont see it on the screen
	- `&> /dev/null`
		- ***just trash errors into nothingness***
		- useful in bash scripts when you do not want the usual error log, but write your own error handler

## |

- `|` pipe, get stdout of a command and make that the stdin of another command
	- `la /etc | less`

#### tee

- `| tee` split the output stream
	- `la | tee output.txt` 
		- will show result of la in the terminal and also write it to output.txt
	- `tee -a` 
		- flag a will append instead of overwrite
	- `./run_experiment.sh 2>&1 | tee experiment.log` 
		- will let you see output of your process live on the screen but also write it to a log file for later review

#### xargs

> Reads input *from* *stdin* (usually a list of lines or words) and ***turns that input into arguments*** for another command.

- `| xargs` 
	- example:
	- `ls -1 | fzf | xargs cat`
- `-r` 
	- Don't run if there is no input.
- `-I{}` 
	- “For each input line, replace `{}` in the command with that line.”
	- `printf "one\ntwo\nthree\n" | xargs -I{} echo "item={}"`
- `-t`
	- *print command* before running it
	- neat for debugging
- `-d`
	- set ***custom delimiter*** (*default is any whitespace*)
	- `-d '\n'`
		- commonly used to split on newlines as a replacement to `-I{}` 
		- then instead of running that command each time per line
		- it will be run once with multiple arguments separated by a whitespace
		- this is usefull only for commands that accept multiple arguments like `git diff`
- `-n N`
	- use N arguments per command
- `-0`
	- split on null bytes
	- ?
- `-P N`
	- run N commands in parallel
	- ?
- examples:
	- assuming output list of path strings from selection in fzf: 
		- `xargs -r -I{} git diff "{}"`
		- this could be simplified with a different flag
		- `-d '\n` 
			- split by new line
		- `xargs -r -d '\n' git diff`

## grep

> "*Global Regular Expressions Print*"
> 
> You will be commonly using it after pipe to find keywords in results of other commands. Grep works line-by-line.
> 
> But it can be also used on specific files or entire directories.

- `grep "error" logfile.txt` 
	- prints all lines containing error in file logfile.txt
	- if it is not used in a pipe, then grep "targets" must be at the end of grep command
	- *you can target multiple whitespace separated files AND directories*
- `-E "your_regex_pattern"` 
	- *you can use regex for the patterns*
	- use extended regex, otherwise you need to escape every special character
	- ***don't forget the quotation marks***
- `-F`
	- *fixed string*
	- good for searching for paths, so that you don't need to escape characters
	- like `grep -r -F ".nvm/versions/node/v24.11.1/bin"`
	- *this disables regexes*
- `-i` 
	- case-insensitive search
- `-v` 
	- invert match, only show the lines not matching
- `-r` 
	- recursive search in directories
	- then `grep -r "pattern" ~/.config` pointed at the *.config directory* will also recursively dig through all directories within
- `--include="*.log"` 
	- only grab from .log files
- `-l` 
	- show only filenames that match
- `-w` 
	- match whole words only
- `-A NUM` 
	- show NUM lines after match
- `-B NUM` 
	- show NUM lines before match
- `-C NUM` 
	- show NUM lines after and before match
- `--color` 
	- highlight matching text

# 🧼 coreutils

> OK, not all of them are a part of actual [GNU Core Utilities](https://en.wikipedia.org/wiki/GNU_Core_Utilities), but all this is the stuff that you really need to know.
## basics

- `man ls` 
	- manual page of a given program
	- can also be usually accessed like `ls --help`
	- there are better alternatives described here [[🪤 packages#how? what?]]
- `pwd` 
	- print current dir
- `cd` moving around dirs
	- `~` home dir
	- `-` previous dir
	- actually not a part of coreutils!
- `ls`
	- `-l` detailed with permissions
		- by default will show modification date
		- `-u` will show access time
	- `-a` also hidden files
	- `ls -la /etc` 
		- you can also point it to another direction than the one you are currently in
	- see [[🪤 packages#lsd]]
- `touch meow`  + timestamps!
	- when *meow* does not exist it will create a file called meow
	- https://www.gnu.org/software/coreutils/manual/html_node/touch-invocation.html
	- actually more interesting than it would seem
	- if file *meow* exists, touch will instead set its timestamps to current time
		- see the notes on [[#file timestamps]]
		- *-a* ... 
			- Change ACCESS time (atime)
		- *-m* ...    
			- Change MODIFICATION time (mtime)
- `stat filename`
	- will display info about a file 
		- this is useful to check [[#file timestamps]]
	- `--file-system`
		- will instead display info about the filesystem where that specific file is located
- `file banana.jpg` 
	- will tell you what type of a file banana.jpg is, because in linux filenames are not required to represent the contents of the file
- `cat` 
	- prints the content of a file
	- see [[🪤 packages#bat]]
- `less` 
	- prints the contents of a file in a more manageable way for longer files
	- `g/G` beginning end of the file
	- `/search` search for words in the file
- `!!` 
	- run the last ran command
	- `sudo !!` useful when you forgot to run previous command with required sudo permissions
- `history` 
	- numbered history of commands
	- `!184` 
		- run the command numbered as 184
	- `!184:p` 
		- prints the command numbered as 184 and lets you edit it before you run it
	- `!git` 
		- runs most recent command that started with git
		- pressing `Tab` will fill with latest `git` command you cast and let you edit it before running
	- `history | grep apt` 
		- lists all commands in history that contain 'apt'
- `cp mycoolfile /home/pete/Documents/cooldocs` 
	- copy files to given location
	- `cp *.jpg /home/pete/Pictures` 
		- using * wildcard to move multiple files
	- `cp -r Pumpkin/ /home/pete/Documents` 
		- using -r flag to copy directories with their contents recursively, THIS WILL OVERWRITE
	- `-i` 
		- interactive flag that will prompt you before you overwrite file
- `mv` 
	- move/rename
	- `mv oldfile newfile` rename file or directory
	- `mv file2 /home/pete/Documents` move file
	- `mv file_1 file_2 /somedirectory` move multiple files
	- `-i` interactive move otherwise it will be overwriting
- `mkdir` 
	- make directory
	- `mkdir books paintings` make multiple directories
	- `mkdir -p books/hemmingway/favorites` create dir with subdirectories
- `rm` 
	- remove
	- `-f` 
		- dangerous, force
		- remove write-protected files
	- `-r` 
		- recursive
		- for directories
- `which` 
	- locate a program in user's path

## extended
### du

> ✨ disk usage
> outputs sizes of subdirectories inside your current directory.

> 🔥 *du shows “logical size” → not “real disk consumption” when dealing with:*
> 
> - loop-mounted filesystems (Snap packages → `/snap`)
> - bind mounts
> - some other specific cases
>   
> ✨ so it won't be perfect for seeing what is eating up disk consumption on root/system partition, but it will be great for /home

- `du -h` 
	- with human-readable format
- `du -h /path/to/dir`
	- you can also point it to a specific dir
- `du -h --threshold 1G`
	- useful for finding large files that take up space
- `--all`
	- also hidden files
- `--max-depth=1`
	- setting it to 1 can help you see what takes most space in given location
- examples:
	- `du -h --max-depth=1 ~ | sort -h`
		- see what takes most space in your home directory
	- `du -h --max-depth=1 --threshold 1G ~/.cache`
		- or see whats heavy in .cache:
```python
1.2G    /home/merryangel/.cache/ms-playwright  
2.2G    /home/merryangel/.cache/JetBrains  
5.6G    /home/merryangel/.cache/kopia  
12G     /home/merryangel/.cache
```


### find

- use cases:
	- `find ~ -name linux.md` search for linux.md file in home dir
	- `find ~ type d -name sanctuary` search for sanctuary dir in home dir
	- `find ~ -name linux.md -exec less {} \;` exec command less on each file found, {} is replaced with the path of the found file
	- `find ~/Projects/sanctuary/sanctuary -type d ! -perm -u+rwx` 
		- find directories that are not writeable
	- `find . -type f -name "*.cs" -exec dos2unix {} +`
		- convert CRLF to LF files marked by find
	- `find . -type f | wc -l`
		- count how many files there are in your current dir

## extracting archives

### unzip

```bash
# Extract to current directory
unzip file.zip

# Extract to specific directory
unzip file.zip -d /path/to/extract/

# List contents without extracting
unzip -l file.zip
```

### tar

- [ ] TODO, what is with all those tar extensions
```bash
# Extract .tar
tar -xf file.tar

# Extract .tar.gz or .tgz
tar -xzf file.tar.gz

# Extract .tar.bz2
tar -xjf file.tar.bz2

# Extract to specific directory
tar -xzf file.tar.gz -C /path/to/extract/

# List contents
tar -tf file.tar.gz
```

### 7z

```bash
# Extract
7z x file.7z

# Extract to specific directory
7z x file.7z -o/path/to/extract/
```

# 📖 text-fu

## crucial

#### cut (basic extraction)

> `cut` extract specific section of **each line** from a file or stdin
> 
> it has limitations but it is way faster than awk or sed

- `-d,` 
	- setting the delimiter to a comma (defaults to a tab if -d isn't set)
	- 🔥 cut only works with single-character delimeters
- `-d"|"` or `-d" "` 
	- if you want to use some shell special characters as delimeters you must surround them with "", same with a white space
- `-f`
	- field flag
	- `-f2` selects second field from the line split by the delimeters
	- `-f1,3,5` selects first, third and fifth fields
- `-c`
	- cut by character positions in line instead of by a delimeter  `-d`
	- `-c3`
		- get only third character
	- `-c3-`
		- from third character to the end
		- 🐱 super useful
	- `-c-3`
		- up to third character
	- `-c3-9`
		- from third to ninth character
- lil examples
	- `cut -d',' -f2 file.csv` will extract a second columnt from given csv file
	- `cut -d':' -f1 /etc/passwd` extract usernames from /etc/passwd
- **cut vs awk**
	- awk uses 'any amount of whitespaces' as delimiter by default
	- you will often face places like `aux grep` when columns are separated by multiple spaces
	- you will not always have access to awk
	- you will need this command to squeeze whitespaces before you cut
	- `tr -s ' '`

#### sort

> Sort lines of text files or of stdout

- `sort -n` 
	- ✨ sort numerically instead of the defauly alphabetic order
- `sort -k2` 
	- ✨ sort by specified field/column (here second)
	- `soty -k2 -t,` 
		- specify the delimeter when using `-k`
		- it defaults to a white space character or tab
- `-h`
	- compare human readable numbers
	- *like 10M or 5G*, so it is good for sorting results of [[#du]]
- `-u` 
	- remove duplicates
- `-f` 
	- ignore case when sorting
- `-r` 
	- reverse
- examples:
	- `cut -d':' -f1 /etc/passwd | sort` 
		- obviously it can also work with pipes
#### uniq

- `uniq` unique
	- lets you remove duplicates across lines of code, it only removes duplicates on adjacent lines so you have to sort the file before
	- `-c` fill print how many occurences of a given line
	- `-u` will only print the unique lines
	- `-d` will only print the lines that were duplicated

#### wc

- `wc` word count
	- `-l` shows the number of lines in a file
	- `-w` number of words
	- `-c` number of bytes

#### nl

> number the lines from a file or stdin

- `echo $PATH | tr ':' '\n' | nl`

#### printf

> Apply formatting to a string.

- `printf '%s\n' "${list_data[@]}"`
	- assuming `list_data` being an array of strings
	- this will apply formatting to each of its elements
	- adding `\n` at the end of each
	- and *returning a single string*
	- like: `printf '%s\n' "apple" "banana" "cherry"`
	- would return here: `"apple\nbanana\ncherry\n"`

#### tr

> super neat for little transforms

- `echo $PATH | tr ':' '\n'`
	- will list your $PATH in nicely readable newlines
- `echo "hello 123 world" | tr -d '0-9'` -d for delete specific characcters
- `echo "hellooo	 world" | tr -s 'o '` -s for squeeze specified characters, here o and a white space
- `echo "hello 123 world $1" | tr -cd 'a-zA-Z '` -c for complement of the set, here only letters and white spaces will remain
- `echo "Hello World" | tr '[:lower:]' '[:upper:]'` change case to upper

## situational
#### join (-ing files based on common field)

- `join` lets you join different files based on a common field 
	- (this is **not** an opposite of a `split`)
	- it is a bit related to `paste` but where paste joins line by line, `join` merges files based on a common field (*like a database join*)
	- **it requires a sorted input on the join field**

#### paste

- `paste` merge lines of files or stdin
	- `-d" "` will use space as a delimeter
	- `paste -d" " file1.txt file2.txt` will merge two files line by line, combining their lines by a white space, you could just aswell do that with three or more files
	- `-s` will instead combine all serially into a *single line*
	- `-` you can use paste with input from pipes
		- `cut -d: -f1 /etc/passwd | paste -s -d, -` will get the usernames from passwd and combine all the lines with a ,
- `head` will show you first 10 lines of a file
	- `-n15` will show first 15 lines
- `tail` like head but last 10 files
	- `-f` will follow the file as it grows / changes are made to it
- `expand & unexpand` rewrite the file replacing tabs into whitespaces or the other way around

#### split (for files, not strings per se)

> Splits a file into more files, lets you specify the splitting by file size or number of lines.

> Notice it is `cat` that is actually an opposite of `split` and neither `join` or even `paste`, see:
> 
> `cat split_part* > reconstructed.txt`

- `split` 
	- `split -b 10M bigfile.txt chunk_` split by size into files prefixed with chunk_
	- `split -l 1000 bigfile.txt part_` split by 1000 lines going into each new file
	- `-d` if you want numeric suffixes for the created files

# 🗿 users and groups

- each user has their own home directory
- each user has an ID, `UID`
- users are on the system to run processes with different permissions
	- thus users don't have to be humans and most of them on a single computer likely won't be
	- such system users will likely have their `UID` below 1000
	- your default personal user id will likely equal to 1000

###  `/etc/sudoers` 

- [ ] how to change root password?

> Users with sudo permissions aka. `superuser-do`
> 
> *System administration is almost **always managed by groups**, **not individual users**.*
> 
> Because it's much easier to add or remove a user from the `wheel` group than to edit the sensitive `/etc/sudoers` file every time.

- *user privieges* are defined as:
	- `root    ALL=(ALL:ALL) ALL`
	- this means user `root` has all privileges
- *group privileges **are prefixed with %***:
	- `%wheel  ALL=(ALL:ALL) ALL`
	- this means all members of the wheel GROUP have all privileges
- ***NEVER EDIT THIS FILE MANUALLY***
	- this file can only be edited with `visudo` command

#### sudo vs. su

> `sudo` superuser-do

- will let you run a command with root privileges
- only users on the `/etc/sudoers` list are allowed to do it

> `su` switch shell to another user

- `su`
	- without username will by default switch you to the root
	- will require *root password*
	- naturally your root password *is*/***should*** be different than your user password
- `su -`
	- substitute to root ***and*** load that user's environment (like their profile)
	- this gives you a full root login session.
	- [ ] what?
- `su username`
	- subsitutes your identity to a given user
	- you must know their password
### `/etc/passwd`  & user's shell

> information about all user accounts

- note that not every user is like a human-user with home etc.
- example line looks like this `root:x:0:0:root:/root:/bin/bash`
	- 1. username
	- 2. user password, `x` means it is stored in `/etc/shadow` file, a `*` means the user doesn't have login access, blank field means the user does not have a password
	- 3. `UID` notice root has UID of 0
	- 4. `GID` **primary** group id
	- 5. `GECOS field` comments about the user, typically comma separated
	- 6. users home directory
	- 7. 🔥 `users shell` is assigned here
		- so it is here that you will likely replace */bin/bash* with */usr/bin/zsh* for your user
		- *important!* do NOT try to change it manually, see [[#switching shell ie. to zsh]]
### `/etc/shadow`  - user passwords

- stores information about uuser authentication
	- 1. username
	- 2. encrypted password
		- `*` no password, authentication with a password is not possible at all (typically for system and service users)
		- `!` the account is locked, if it is ! with no hash after it then no password hash exist
		- `!*` functionally it is the same as `*` but it can serve as an indicator that it was deliberately disabled
		- all three `*, !, !*` mean that user cannot log in via password
	- 3. last password change date (`since Jan 1 1970`)
	- 4. min. password age (days untill user is able to change password)
	- 5. max. password age (max number of days before user HAS to change their password)
	- 6. password warning period
	- 7. password inactivity period (how many days *after* password has expired that it can still be used)
	- 8. expiration date
	- 9. reserved field foor future use
### `/etc/group`

***Piece of history** & setting your su-privileged group name to* "***wheel***":

> The name comes from ***1960s-1970s computing slang***.
> 
> At institutions like MIT, the most powerful system administrators had access to a room with a literal ***"big wheel"*** of tape drives and controls. 
> 
> Getting a spot "on the wheel" meant you had the highest privileges. 
> 
> The term stuck in the Unix world as the group for users who could `su` (substitute user) to become root.

It's a piece of computing history that's survived because it's unique and memorable.

- [ ] how to add/remove user to a group
- [ ] how to change group name

- `groups` 
	- will list all the groups your active user is in
- `groupadd`
	- will create a new group
- `groupdel`
	- delete a group
- `lpadmin:x:114:merryangel`
	- 1. group name
	- 2. group passwd
	- 3. group ID
	- 4. list of users

### /etc/hostname & /etc/hosts

> https://serverfault.com/questions/228102/hostnames-what-are-they-all-about
> 
> ***/etc/hosts, why?*** https://en.wikipedia.org/wiki/Hosts_(file)#Purpose

```bash
### **Why `derp`, my system installation original hostname not in `/etc/hosts`**

1. **It Was a Transient Hostname**: When you first installed Arch, you might not have set a static hostname explicitly. In this case, `systemd` (via `systemd-hostnamed`) will default to using `localhost` or, if the network's DHCP server provides a hostname (often based on your machine's MAC address or the router's whim), it uses that. This temporary name was `derp`, but it wasn't "officially" registered in your system's static configuration files (`/etc/hostname` and `/etc/hosts`).
    
2. **`/etc/hosts` is for Static Mappings**: The `/etc/hosts` file is for _static_, administrator-defined mappings between IP addresses and hostnames. It's not automatically updated by transient network events. Since `derp` was a transient name, it was never written to this file.
```

- `hostnamectl`
	- will print your hostname
- `sudo hostnamectl set-hostname newmeowname`
	- will change your hostname to `newmeowname`
	- if you do it your hostname in /etc/hosts will be updated
	- `hostnamectl` will also show new name
	- 🔥 but you can't just stop here after doing that
	- you also now need to update `/etc/hosts` and add a line
		- `127.0.1.1    newname.localdomain    newname`
		- the amount of whitespaces doesn't matter
		- [ ] i still don't really get why do you need to do it
		- [ ] why does it have to be `.localdomain`
		- [ ] how is this ip chosen
### user management tools

#### sudo 

- `sudo useradd bob` creates user bob
	- `-m` will create home dir for that user
	- `-s /bin/bash` specify shell for that user
- `sudo userdel bob` deletes bob user
	- `-r` also permanently destroys his home folder with all its files, dangerous
- `sudo passwd bob` set password for the given user

# 📂 permissions & file attributes

#### drwxr-xr-x

Example of permissions through `ls -l`:

```bash
drwxr-xr-x merryangel merryangel 4.0 KB Fri Nov 14 08:55:14 2025  .var  
.rw------- merryangel merryangel 9.9 KB Fri Nov 14 09:08:11 2025  .viminfo
```

- _d or ._ 
	- single first character
	- these mean directory or file
- _rwx_ - read, write, execute
	- *rwx* means max permissions for the corresponding recipient
	- _-_ dash means that permission is not granted
- then there are *three sets* of permissions `rwx` for each:
	1. user
	2. group
	3. anyone
#### modifying permissions 

#coreutils chmod

> symbolic mode
> `chmod u+x myfile`

- how to read it
	- `**u**` user
	- `**g**` group
	- `**o**` others
	- `**+ / -**` add / remove permissions
	- `**r/w/x**` read / write / execute
- examples
	- `chmod ug+x myfile`
	- `chmod o-x myfile`
	- `chmod +x somefile` neat, will add x for everyone

> numeric mode
> `chmod 755 myfile`

- how to read it
	- `**4**` read
	- `**2**` write
	- `**1**` execute
- thus common values will be:
	- `**7**` all permissions
	- `**6**` read/write
	- `**5**` read/execute
	- `**4**` read
	- `0` fuck off

#### modifying ownership

#coreutils chown & chgrp

- `chown kitty file`
	- will give **kitty** user ownership of the file
- `chown kitty:besties file`
	- will change file's owner to kitty and group owner to bestios
- `chgrp besties file`
	- will **change the group associated with** that file

#### umask
> Manage the read/write/execute permissions that are masked out (i.e. restricted) for newly created files by the user.  
> More information: https://www.gnu.org/software/bash/manual/bash.html#index-umask.

TODO kindof situational, no? prob important for sysadmin

**Formula: `final_permissions = default_permissions & ~umask`**

#### file timestamps

There are three+1 Timestamps:

- *Access time (atime)* 
	- when file was last read
	- can be changed with `touch -a`
- *Modification time (mtime)* 
	- when file content was last changed
	- can be changed with `touch -m`
- *Change time (ctime)* 
	- when file metadata was last changed
		- permissions, ownership, etc.
		- etc! it means actual *timestamp* change by using `touch`!
		- this means if someone tries to hide a recent file modification date using touch, the *ctime* will be updated to the current and unchangeable time aswell!
	- **CANNOT be changed manually**
		- it is immutable by design!
		- it updates automatically
		- always current, always reflects the most recent metadata change
		- it is commonly an audit trail
		- **The ctime auto-update is handled by the Linux kernel's filesystem layer.** It's a fundamental feature of Unix/Linux filesystems.
		- There's **NO system call** to set ctime directly.
		- Each filesystem (ext4, btrfs, XFS, etc.) implements this, but the kernel VFS (Virtual File System) layer enforces the behavior.
- *Birth time (btime)* 
	- when file was created (not all filesystems)

You can check them with:

- `stat filename`
- `ls -l`
	- modified time
- `ls -lu`
	- access time
- `ls -lc`
	- change time (permissions)

Security significance of ctime:

```bash
# Suspicious pattern: mtime older than ctime
# Means file was modified, then metadata changed separately
find / -type f -mtime +1 -ctime -1 2>/dev/null
# Files modified >1 day ago but metadata changed today

# Common attack pattern:
# 1. Attacker modifies system binary (changes mtime)
# 2. Attacker uses 'touch' to set old mtime (change in timestamp which is a part of file metadata also changes ctime!)
# 3. Result: mtime looks normal, but ctime is recent
stat /usr/bin/suspicious_binary
# mtime: 2023-01-01  (looks old)
# ctime: 2024-12-15  (actually recent!)
```


That's why in real attacks, sophisticated actors:

- Don't bother hiding timestamps (they know they'll be caught anyway)
- Focus on covering other tracks (logs, network traces)
- Use anti-forensic techniques at filesystem level
- Or just accept that timestamps will betray them
# 📦 package management

### pacman (Arch)

> Official Arch package repository.

- [ ] learn about `/etc/pacman.conf`



***Flags and common usage***:

- `-S` *--sync*
	- used to save packages from repositories
	- and also to update them using queries
	- *sync options*:
		- `-c` *--clean*
			- remove packages that are no longer installed
		- `-y` *--refresh*
			- refresh master package databases defined in `pacman.conf`
			- this should be typically always done when `-u` is used
		- `-u` *--sysupgrade*
			- upgrade all packages that are out of date
		- `-l` *--list*
			- list all files owned by a given package
		- `-i` *--info*
			- info on a given package
	- *examples*:
		- `sudo pacman -Syu`
			- update system
			- ***do this regularly!***
		- `sudo pacman -Syu openssh`
			- refresh package databases, upgrade all packages and then download openssh
		- `sudo pacman -Scc`
			- clear cache to free space
			- [ ] not sure how it works, double `cc` is intentional
- `-R`
	- remove package
	- *options*:
		- `-n`
			- also clearing cache
		- `-s`
			- also unneded dependencies
	- *examples*:
		- `sudo -Rns zsh`
			- if you have packages that depend on `zsh` this will fail!
			- you need to uninstall them first
- `-Q` *--query*
	- query local package database 
	- alone will simply list all installed packages
	- *options*:
		- `-i`
			- display info on a given package
		- `-q`
			- quiet
			- this will list the installed packages `without their versions`
			- this is usefull when you want to just grab that whole list to later use it in an installation script to automate the process of installing all the packages that you are used to have on your arch pc
		- `-e`
			- limit the listing to only *explicitly installed* packages
			- like it excludes their autoinstalled dependenies
		- `-d`
			- opposite of `-e`
			- list only packages installed as dependencies
			- often combined with `-t`
		- `-t`
			- unrequired
			- Restrict or filter output to print only packages neither required nor optionally required by any  currently installed package. 
			- Specify this option twice to include packages which are optionally, but not  directly, required by another package.
			- this is useful when you forget `-ns` when you do `-R`
		- `-l packagename`
			- list all the files owned by the given package
			- usful to like find where its commands or default configs are located
	- *examples*:
		- `pacman -Qqe`
			- quietly list all explicitly installed packages
			- you can also add `| wc -l` to see how derailed you currently are
		- `yay -Ql openssh`
			- list all files owned by openssh package
			- `yay -Ql opennssh | grep bin`
				- will most likely let you see commands provided by this package
		- `pacman -Qo $(which ssh)`
			- zu welchem Paket gehört ein bestimmtes Kommando wie ssh
- `-F` *--files*
	- query files database
	- *file options*:
		- `-y`
			- refresh package filedatabase `repo.files`
		- `-l`
			- list files owned by the queried package
	- *ex*

***Examples***

- pacman -Qm

- `pacman <package partial name>`
	- lets you search for packages related to your given keyword
- *Listing files in package:*
	- `pacman -Fl packagename`
		- zeigt alle Dateien im ***remote*** Paket
		- you will probably need `-Fyl` before it works
		- gleich für yay: `yay -Fl packagename`
	- `pacman -Ql packagename`
		- same for an already installed package
-
	- 


***Mirror lists***:

> Use `reflector` to fetch and sort Arch mirrorlists.
> 
> Normally you do this during installation.

- `cat /etc/pacman.d/mirrorlist`
	- see your current mirror lists
	- something like:

```
######## Arch Linux mirrorlist generated by Reflector #########  
  
# With:       reflector --country DE --latest 5 --sort rate --save /etc/pacman.d/mirrorlist  
# When:       2026-01-14 19:32:15 UTC  
# From:       https://archlinux.org/mirrors/status/json/  
# Retrieved:  2026-01-14 19:32:02 UTC  
# Last Check: 2026-01-14 19:22:34 UTC  
  
Server = https://de.arch.mirror.kescher.at/$repo/os/$arch  
Server = https://frankfurt.mirror.pkgbuild.com/$repo/os/$arch  
Server = rsync://berlin.mirror.pkgbuild.com/packages/$repo/os/$arch  
Server = https://berlin.mirror.pkgbuild.com/$repo/os/$arch  
Server = rsync://de.arch.mirror.kescher.at/mirror/arch/$repo/os/$arch
```

- `reflector --country DE --latest 10 --sort rate --save /etc/pacman.d/mirrorlist`
	- [ ] todo: what is the reflector
	- [ ] what is the use of `rsync here`
### yay (Arch)

- [ ] what is the whole AUR thing about?

***Examples***:

- `yay keyword`
	- ***NEAT, USE THIS***
	- search *package database* for a keyword *from both repos and AUR*
	- pacman doesnt have it, maybe only something like `pacman -Si packagename` but if it isnt in db it will give nothing
	- `yay keyword` will actually look around various packages not just in their names, but in their tags, descriptions if they have some relation to that keyerd
- `yay -Syu`
	- like pacman, proper refresh and upgrade
- `yay -S packagename`
	- install specific package
- `yay -Rns`
	- remove package, clear cache and remove dependencies

***Emergency downgrade***:

- `downgrade`
	- quite important, you might have to install it manually with `yay -Syu downgrade`
	- if the latest version of a package you are using got broken and you want to use an older version that worked for you you can do:
	- `sudo downgrade openssh`
	- if you didnt clear your cache recently, the one recent version that worked properly might be in your cache and will be listed after you run `downgrade` on that package
### dpkg (Debian und Ubuntu)

- `dpkg -l` 
	- list of all installed packages
- `dpkg -L xdg-utils` 
	- will list all files installed by xdg-utils package
	- notice that all the stuff it puts in `/etc/bin` are the actual commands it provides
- `dpkg -S ${ssh}`
	- will tell you what package owns this command
- `sudo dpkg -i xxx.deb`
	-  install package from `.deb` file
### apt (Ubuntu)

> apt is the package manager of Ubuntu, it is built on top of `dpkg`, which is a core debian package manager

> ***Wenn du willst untersuchen, welche Kommandos durch ein Paket verfügbar gemacht werden:*** dpkg -L paketname

- resolves dependencies
- downloads and uses `.deb` packages
- calls `dpkg` to actually install them
- use system libraries (contrary to the self-contained snap)
- `apt search <package keyword>`
	- lets you search repositories for package with a given keyword
- `apt list`
	- **--installed**
		- will list all installed packages, with wc -l on Ubuntu it gave me *98663* packages! i wonder how many will i have on fresh Arch install
	- **--upgradeable**
- `apt update`
	- it doesnt update anything basically
	- it loads:
		- new package versions
		- [ ] security updates, so it does update something?
		- [ ] dependencies, what does that mean?
- `apt upgrade`
	- that is what actually upgrades packages
	- check first with **list --upgradeable**
	- **-s oder --dry-run** 
		- dry run
- `apt show <packagename>`
	- details to a specific package
- `apt install <packagename>`
	- `-y` automatically answer yes to prompts
	- `-q` quiet mode, less output
	- `--reinstall` reinstalls a broken package
	- `--fix-broken` attempts to fix broken dependencies
- `apt install ./package.deb`
	- installing a local `.deb` package
- `apt remove packagename`
	- `--purge` also deletes config files
- `apt autoremove` removes unused dependencies
	- `--dry-run` better to always run first to review what will be removed
- `apt list --installed` will get u a list of installed stuff
### snap (Ubuntu)

- uses `.snap` packages
	- compressed, self-contained filesystem images that mount read-only under /snap/package/
	- bundles its own llibraries
	- ! auto updates in background
- `snap info rider`
- `snap install packagename --classic`
	- **Strict confinement (default)** → app can only access certain folders, interfaces, and system resources.
	- **Classic confinement (`--classic`)** → app behaves more like a traditional Linux program, with full filesystem access.
- `sudo snap refresh rider --channel=2025.1/stable`
	- `refresh` reinstall a package, in this case to a specific version/channel as listed through `info`
- 🔥 has bad rep because ubuntu kindof forces it on people and you cant stop the updates
### archives

- `tar` archiving utility for `[.gz|.bz2|.xy]` files
	- `tar xf file --directory path/to/where/unpack` e[x]tract [f]ile to target dir
- `unzip`
	- `-d path/to/output` 
### AppImage
- `AppImage`
	- `chmod +x SomeApp.AppImage` after downloading you need to give it executable permissions
	- `sudo apt install fuse libfuse2` you need fuse to run AppImages
	- `./SomeApp.AppImage` then you just run it directly
	- https://docs.appimage.org/user-guide/index.html

### deb
- Since Ubuntu 20.04+, you can install local `.deb` files safely with `apt`, which handles dependencies automatically
	- `sudo apt install ./package.deb`

# 🧭 system
- `ctrl+alt+f3` starts up a virtual terminal tty3, there is also 4,5 and 6
	- modern linux systems usually run multiple "screens" in parallel
	- this is good when your gui gets stuck
	- you can login into each one independently
	- each `tty` runs its own session, you can login to different users across them

## boot

- [How linux boots?](https://youtu.be/EjrAzulPsT4?si=0f7IuMzQ25NggNpv)

- bios vs. uefi
- grub


```
When you press the power button:

1. The kernel loads.
2. The kernel starts **systemd**.
3. **systemd** starts all other system services (like networking, login managers, and Syncthing).
   
Locations of systemd files:
/usr/lib/systemd/system/     ← default system services
/etc/systemd/system/         ← custom or overridden services
~/.config/systemd/user/      ← user-specific services
```


## 🥜 kernel

#### dmesg

>  Writes kernel messages to stdout.

- [ ] what is the relation of this thing to journalctl
#### lsmod

>List all currently loaded kernel modules

#### modprobe

> Add or remove modules from the Linux kernel.  
  
 - Pretend to load a module into the kernel, but don't actually do it:  
`   sudo modprobe --dry-run module_name  `
  
 - Load a module into the kernel:  
`   sudo modprobe module_name  `
  
 - Remove a module from the kernel:  
`   sudo modprobe --remove module_name  `
  
 - Remove a module and those that depend on it from the kernel:  
`   sudo modprobe --remove --remove-holders module_name  `
  
 - Show a kernel module's dependencies:  
`   sudo modprobe --show-depends module_name`

#### PAM


**PAM (Pluggable Authentication Modules)** is a core Linux system layer that handles authentication for many services (login, SSH, sudo, etc.). Think of it as a "switchboard" for login requests.

- **What it does**: When you type a password for SSH, `sshd` doesn't check it directly. It hands the request to PAM. PAM then runs through a set of configured rules (modules) to decide if the login should succeed (e.g., check password, enforce 2FA, restrict login times).
    
- **The SSH Setting (`UsePAM`)**: When `UsePAM yes` is set in `/etc/ssh/sshd_config`, SSH defers to the PAM system for password and account validation. When it's `no`, SSH handles simple password checks itself.
    
- **Key Point for You**: Since you are **disabling all password logins** (`PasswordAuthentication no`), the `UsePAM` setting becomes less critical for SSH. You can usually set it to `no` safely. However, if you plan to add **two-factor authentication (2FA) via SSH in the future**, you will likely need PAM (`UsePAM yes`).

## 🗻 system file-structure

>  Full print from `tree -L 1 /`.
>  
>  Lines marked with `bool` will be most likely of more interest to you. `bool` just because it is nicely coloured.
```c
├── bin -> usr/bin  
├── bin.usr-is-merged  
├── boot  
├── cdrom  
├── dev   bool DEVICES
├── etc   bool CONFIGS
├── home  
├── lib -> usr/lib  
├── lib64 -> usr/lib64  
├── lib.usr-is-merged  
├── lost+found  
├── media  
├── mnt  
├── opt  
├── proc  
├── root  
├── run  
├── sbin -> usr/sbin  
├── sbin.usr-is-merged  
├── snap  
├── srv  
├── sys  
├── tmp  
├── usr
│   ├── bin bool WHERE ALL BINS ARE MOUNTED
└── var
```

>  Here sorted by how likely you are to interact with it:
- `/usr/bin`
	- all your installed bins will most likely be here
	- `/bin /sbin` will be usually ***mounted*** to here



##  🗻 systemd

> ***Init system*** used by most modern Linux distributions
> 
> Basically the ***manager that starts and supervises all services*** when your computer boots.
> 
> You can see your services here: `lsd /etc/systemd/system`

#### systemctl

> `systemctl` the tool you use to control `systemd`

- `systemctl status servicename`
	- if you cant see it there, try: `systemctl status --user syncthing`
- `systemctl start/stop/restart servicename` 
	- start/stop/restart service
- `systemctl enable/disable servicename` 
	- enable/disable service from starting automatically at boot
- `--user` 
	- ***important flag*** it will run that service as your normal user after login
		- otherwise it is loaded as root before any user logins, potentially dangerous
	- the `--user` flag is more about *organizing* services than being the primary security boundary. The real security comes from the Unix permission model itself!
	- examples:
		- `systemctl start syncthing` 
			- without sudo fails immediately 
			- [ ] no permission to talk to system bus) - system bus huh?
			- `systemctl --user start syncthing`
				- runs as user (*safe, no sudo required*)
- `systemctl --failed` 
	- for debugging if there were some services that didnt  correctly run at boot
- `systemctl list-units`
	- will list units that are ***currently in memory***
#### journalctl

> Query systemd journal.

***Flags***:
- `-u` *--unit*
	- shows messages for a specified systemd unit (such as service unit)
	- see `systemctl list-units` to see 
	- example:
		- `sudo journalctl -u NetworkManager`
- `--since / -S`
	- takes dates but also things like "*yesterday, today, 5 minutes ago*", example:
		- `sudo journalctl -u duckdns.timer --since today`
	- there is also `--untill / -U`
- `-f`
	- will keep the journal open and update when some activity happens

***Examples***:
- `journalctl --boot --priority 3`
	- all messages with *error priority level (3)* from this boot 

#### timers

> List all active timers with: `systemctl list-timers --all`
#### custom service (duckdns.org)

***An example based on setting up a timer service to update duckdns IP***:
- create two files:
	- `sudo nvim /etc/systemd/system/duckdns.service`
	- `sudo nvim /etc/systemd/system/duckdns.timer`
```bash
# duckdns.service
[Unit]
Description=DuckDNS update

[Service]
Type=oneshot
ExecStart=/usr/bin/curl -k -o /var/log/duckdns/duck.log "https://www.duckdns.org/update?domains=YOURDOMAIN&token=YOURTOKEN&ip="

#duckdns.timer
[Unit]
Description=Run DuckDNS update every 5 minutes

[Timer]
# this means it will run every 5 minutes
OnCalendar=*:0/5
Persistent=true

[Install]
WantedBy=timers.target
```
- ***IMPORTANT NOTICE***, you don't need to set the name of service that needs to be targetted by the timer
	- *systemd will assume that the service will have the same name as timer*
	- like you did above `duckdns.service` & `duckdns.timer`
	- this is a common standard
- then you just run:
	- `sudo systemctl enable duckdns.timer`
	- `sudo systemctl start duckdns.timer`
	- ***don't enable the service itself***, timer is enough in this case
- you can now verify that everything works with:
```bash
❯ sudo systemctl status duckdns.timer  
● duckdns.timer - Run DuckDNS update every 5 minutes  
    Loaded: loaded (/etc/systemd/system/duckdns.timer; enabled; preset: disabled)  
    Active: active (waiting) since Mon 2026-02-16 11:59:11 CET; 3s ago  
Invocation: 9c0512ccfa8247e99c5c08dfcbe9a64e  
   Trigger: Mon 2026-02-16 12:00:00 CET; 44s left  
  Triggers: ● duckdns.service  
  
lut 16 11:59:11 doomhamster systemd[1]: Started Run DuckDNS update every 5 minutes.
❯ sudo systemctl status duckdns.service
○ duckdns.service - DuckDNS update
     Loaded: loaded (/etc/systemd/system/duckdns.service; static)
     Active: inactive (dead)
TriggeredBy: ● duckdns.timer
```
- it is OK that `duckdns.service` says it is dead, notice the `TriggeredBY`
- you can now watch the activity of the timer with:
	- `sudo journalctl -u duckdns.timer -f`
## 🖥️ devices

### lsblk

> List information about devices.

- `lsblk -f --ascii` neatly print info about **filesystems**
	- [ ] what the hell are all those loops and squashfs things listed there on my ubuntu
- **-f**
	- output info about filesystems, useful!
- **--ascii**
	- prints output in a nice ascii tree
	- where you can see devices and partitions within it
- **--path**
	- will print full device paths

### lspci

> List all PCI (***Peripheral Component Interconnect***) devices.
> 
> https://en.wikipedia.org/wiki/Peripheral_Component_Interconnect

- `lspci`
	- dump all devices simply
-  `lspci -k`
	- also display *kernel drivers* and modules handling each other
	- so interestingly this way you can check if you are using your proprietary *nvidia* driver if you have problems with a computer game 😼 

### lsusb

>  Information about USB busses and devices connected to them.

- `lsusb`
	- basic overview
- `lsusb --tree`
	- neat tree view, but kinda other informations
	- can add `--verbose` here aswell
- `lsusb --verbose -s busnumber:devicenumber`
	- will show detailed information on the given bus number and device nuumber
## shell

### what is a shell?

A *shell* is a program that:

- Takes the commands you type (like `ls`, `cd`, `git status`)
- Interprets them
- And tells the operating system what to do

> So it’s your *command-line interpreter* - the layer between _you_ and the _kernel_.

🧩 Common shell examples:

| Shell | Full name                    | Notes                                                                  |
| ----- | ---------------------------- | ---------------------------------------------------------------------- |
| bash  | _Bourne Again SHell_         | Default on most Linux distros; simple and widely supported             |
| zsh   | _Z Shell_                    | More modern shell with extra features (autocomplete, globbing, themes) |
| fish  | _Friendly Interactive SHell_ | Very user-friendly, but less POSIX compatible                          |
| sh    | _Bourne Shell_               | Very old, minimal, standard on UNIX systems                            |
- [ ] what does it mean POSIX?
### changing user shell

***What is my current shell?***

- `echo $SHELL`
	- /usr/bin/zsh
	- /bin/bash

***Temporarily switch shell***

- `bash`
- `zsh`

***Switch shell permanently***

- your user shell info in */etc/passwd* needs to be updated, it should never be edited manually, use instead:
	- `chsh` 
		- change user login shell, part of `util-linux`
	- `chsh -s $(which zsh)` 
		- set zsh to a default shell

### shell configuration

> This is done by editing `.zshrc` or `.bashrc` files in home directory.

Important things that are done there:
- Adding things to your *$PATH*
- Creating *aliases*
	- btw. you can list all active aliases with `alias` command

#### .zshrc

- [ ] https://thevaluable.dev/zsh-completion-guide-examples/

- Setting shell *theme*
	- You can read more about *Starship* in [[#meow-meow]]

## mounting
### /etc/fstab

> ***File Systems Table*** 
> 
> The fstab file *typically lists all available disk partitions* and other types of file systems and data sources that may not necessarily be disk-based, and indicates how they are to be initialized or otherwise integrated into the larger file system structure.
>
>*The fstab file is read by the mount command, which happens automatically at boot time* to determine the overall file system structure, and thereafter when a user executes the mount command to modify that structure. It is the duty of the system administrator to properly create and maintain the fstab file. 

***See it***:

- `cat /etc/fstab`

***Resources***:

- https://www.redhat.com/en/blog/etc-fstab
- https://en.wikipedia.org/wiki/Fstab
## processes

### tools

#### ps aux
- `ps aux`
	- `ps aux | grep Unity | awk '{print $1" "$11}'`
		- field 11 is process name


#### kill & pkill

- `kill`'
	- kill a process *by PID*
- `pkill` 
	- "  Signal process by name." whatever that means
	- "Mostly used for stopping processes.", yes
	- `-9`
		- forcefully kill all matching processes
		- `pkill -9 process_name`
	- `-f`
		- lets you match full process string, **argv[]**, instead of just the process name 
		- `pkill -9 -f "/Editor/Unity"` 
#### lsof

> List open files and the corresponding processes. 
> Note: Root privileges are required to list files opened by others.  
> More information: https://manned.org/lsof.  

- *tldr use-cases*
	- Find the processes that have a given file open:  
		- `lsof path/to/file`
	- Find the process that opened a local internet port:  
		- `lsof -i :port  `
	- Only output the process ID (PID):  
		- `lsof -t path/to/file`  
	- List files opened by the given user:  
		- `lsof -u username`  
	- List files opened by the given command or process:  
		- `lsof -c process_or_command_name`  
	  - List files opened by a specific process, given its PID:  
		  - `lsof -p PID`  
	  - List open files in a directory:
		  - `lsof +D path/to/directory ` 
	  - Find the process that is listening on a local IPv6 TCP port and don't convert network or port numbers:
		  - `lsof -i6TCP:port -sTCP:LISTEN -n -P`
		  - [ ] lol that seems to be such a specific use case, where is it useful

#### htop

## environment variables & $PATH

`env`
- will list all your environment variables

`echo $SHELL`
- will print the value in *SHELL* environment variable

`echo $USER`
- will print your username

`echo $PATH`
- will print your env paths
- notice that multiple sources will be usually adding to the *PATH* variable
- they have to be separated by ":" character, see:
	- `echo $PATH | tr ':' '\n'`
	- when adding things to the path: *DO NOT FORGET THE ":" SEPARATION*
- you will likely be adding to path through `~/.zshrc`, this will look something like this:
```
export PATH="$PATH:$HOME/.dotnet/tools"
export PATH="$PATH:$HOME/.local/mybinz:$PATH"
```
- you can refer to other env variables like *$HOME* when your env variables are stored somewhere there
- *ORDER MATTERS*, what is earlier on the lists overrides commands that could come later with the same name
	- this means putting custom, user-level-editable paths *before* system commands can be a serious ***security vulnerability***
	- because they could overwrite even commands like `sudo`!





## symlinks
- `ln -s ../relative/from/path to/path` 
	- `-s` makes it a symbolic and not a hard link
	- link is created from the first argument to the second
	- you can use both absolute and relative paths, the above example uses relative links
	- symlinks aren't perfect for git repos due cross compatibility
- `readlink to/path`
	- you can read where the link points to
	- notice the link is obviously placed there where you access it, **where you see the portal to the original directory**
	- `-f` will show absolute path

```
### 🧭 Tip for sanity

Whenever you use `ln -s`, ask yourself:

> “Where will this link **live**?”  
> “Where does it need to **point**?”

If both are relative paths, imagine them as _from the perspective of the new link’s location_.

Thus
$ ln -s ../../Assets/StreamingAssets/BrowserAssetsDist dist
makes sense if you are in location parent to what will be a new dist folder 'containing' what is inside of BrowserAssetsDist
```
## tools

### timedatectl

> Control system date and time.

- `timedatectl status`
- timzeone
	- `timedatectl list-timezones`
	- `timedatectl set-timezone timezone`
- `timedatecrl set-ntp on`
	- enable automatic timedate synnchronization
###  dd 

#coreutils dd 

https://www.gnu.org/software/coreutils/manual/html_node/dd-invocation.html

> dd is used to convert and copy files
> 
> https://blog.kubesimplify.com/the-complete-guide-to-the-dd-command-in-linux
> 
> is commonly named as *disk destroyer*, *data definition*, *disk dump* or *disk duplicator*
> 
> notice the name *disk destroyer*, use this tool cautiously or you can obliterate disks or partitions easily
> 
> use `lsblk -f --ascii` to your help!

#### flags
- **if=/path/asd**
	- source, copy this
- **of=/target/path**
	- target, copy to
- **bs=4M**
	- block size
	- this will specify the size of a copy chunk, so 4M will copy the data step by step in 4MB chunks
- **status=progress**
	- will show its progress as it is working
- **oflag=sync**
	- options are: direct, sync
	- [ ] understand why sync is good and safer for createing a bootable USB
	- sync option will mean that it is ensured every chunk is physically written before continuing
- **skip=100**
	- will skip first 100 bytes of the source
	- [ ] when would i have a use for that?

#### usecases
 1. Creating a Backup of a Linux Disk Partition:
 
> One of the powerful use cases of the dd command is creating backups of disk partitions. This can be particularly useful for system administrators or users who want to preserve the state of their disk partitions. 
> 
> To back up a disk partition, you need to identify the block device associated with the partition, usually represented by a device file in the `/dev` directory.
> 
> For example, to back up the first partition of the disk located at `/dev/sda`, you would use the following command:
 
```bash

dd if=/dev/sda1 of=partition_backup.img

you can then revert this process with:

dd if=partition_backup.img of=/dev/sda1

this can be also done for the entire drives:

dd if=/dev/sda of=hard_drive_backup.img

and

dd if=hard_drive_backup.img of=/dev/sda

```

2. Copying Content from a CD/DVD Drive

```
dd if=/dev/cdrom of=disk_copy.iso
```

3. Compressing Data Read

```
sudo dd if=/dev/sda bs=1M | gzip -c -9 > sda.dd.gz
```

4. Wiping a Block Device
- [ ] how do the methods with random/urandom work, will they simply stop copying and overwriting once the target disks capacity is reached?

```
simple wipe:

$ sudo dd if=/dev/zero bs=1M of=/dev/sda

safe wipe filling disk with random data:

$ sudo dd if=/dev/urandom bs=1M of=/dev/sda

same but slower but more secure:

$ sudo dd if=/dev/random bs=1M of=/dev/sda
```

5. The last usecase deserves its own header:

#### Creating a bootable USB

- `lsblk -f --ascii --path`
	- identify the target USB disk
- `sudo unmount /dev/sdb1`
	- unmount its partition if it is mounted
- `sudo dd bs=4M if=~/Downloads/file.iso of=/dev/sdb status=progress oflag=sync`
	- if the usb is at `sdb`


# 🍙 graphics

## 1. drivers, modesetting

## 2. display server / compositor

>  Xorg is a display server, for Wayland the word is compositor.

***Things it does:***
- Directly talks to the GPU/kernel (via drivers like DRM/KMS)
- Manages windows, their positions, and stacking order
- Handles input from keyboards, mice, touchscreens
- Renders the final screen image (compositing)
- Enforces security between applications

***Example compositors***:
- X.Org (X11 display server)
- kWin (KDE's Wayland compositor/X11 window manager)
	- [ ] how can it be both?
- GNOME's Mutter (gnome's Wayland compositor)

### Xorg

> Xorg is an implementation of the X11 protocol. 
> 
> There are some other implementations, but they are not so commonly used.

- On Arch I used following packages:
	- `xorg-server`
	- `xorg-xinit`
	- `xorg-xrandr`
	- `xorg-xsetroot`
- `startx`
	- starting xorg server
	- this will use your `~/.xinitrc` file
	- example contents with a simple bg and i3
```bash
#!/bin/sh

feh --bg-fill ~/pillarz.ping &

exec i3
```
- doing this automaticall on startup
	- A: Use Display Manager or IDE
	- B: Run it through your login shell
		- https://wiki.archlinux.org/title/Xinit#Autostart_X_at_login
		- *note!* don't use your `.zshrc` but `.zprofile`, you want to run it only on through *login shell initialization file*
## 3. display server protocol

> A ***display protocol*** is the *set of rules* that defines how different components talk to each other.
> 
> The main two protocols here are the older one X11 and Wayland that was designed to replace it.

X11
```
Applications → X11 Protocol → X.Org Server → Kernel/GPU
```

Wayland
```
Applications → Wayland Protocol → Wayland Compositor (Mutter, KWin, etc.) → Kernel/GPU
```
### x11 vs Wayland

- `echo $XDG_SESSION_TYPE`
	- will tell you which one you are on
## 4. compositor

>  You will need a compositor only when your window manager doesn't do compositing on its own. 
>  
>  If you are using a full desktop environment you definitely don't need one.

### picom

>  https://wiki.archlinux.org/title/Picom

## 5. window manager

### i3

## 6. display manager

> Controls the login screen basically?

## Other

### XDG

> **XDG** stands for **Cross-Desktop Group**, part of the **freedesktop.org** project.  It’s not a single program, but a set of **standards and tools** that make different desktop environments (KDE, GNOME, XFCE, etc.) behave consistently.

>These standards define _where and how_ desktop environments store things like:

| Category          | Example                                   | Standard                           |
| ----------------- | ----------------------------------------- | ---------------------------------- |
| App shortcuts     | `/usr/share/applications/firefox.desktop` | **Desktop Entry Spec**             |
| Default apps      | which program opens a URL                 | **MIME/Default Applications Spec** |
| Config paths      | `~/.config`, `~/.local/share`             | **Base Directory Spec**            |
| Icons & themes    | `/usr/share/icons/hicolor`                | **Icon Theme Spec**                |
| Autostart entries | `~/.config/autostart/`                    | **Autostart Spec**                 |
>Below are all part of `xdg-utils`

| Command                  | What it does                                                       |
| ------------------------ | ------------------------------------------------------------------ |
| `xdg-open <file-or-url>` | Opens a file or URL in the user’s default app                      |
| `xdg-mime`               | Gets/sets file associations (like what opens `.png` or `https://`) |
| `xdg-settings`           | Changes higher-level defaults, like the web browser                |
| `xdg-desktop-menu`       | Installs/removes menu entries                                      |
| `xdg-user-dirs-update`   | Manages folders like Documents, Downloads, Pictures                |
>xdg-settings
- `xdg-settings get default-web-browser`
- `xdg-setttings set/check default-web-browser firefox`

>xdg-mime
>Query and manage MIME types according to the XDG standard. 
>More information: https://portland.freedesktop.org/doc/xdg-mime.html.
- `xdg-mime query filetype path/to/file`
	- display the MIME type of a file
- `xdg-mime query default image/png`
	- display the default application of opening png's
- `grep x-scheme-handler ~/.config/mimeapps.list`
	- will list x-scheme-handler MIME apps for you
	- `~/.config/mimeapps.list` holds your **per user MIME apps list**
	- notice all those apps are `.desktop`

> .desktop apps
- these are stored in two locations:
	- `/usr/share/applications/` system-wide
		- 🔥 if you make changes to an app here, it might be overwritten anytime, but if you copy it to the user apps location below, then it will take precedence
	- `~/.local/share/applications/` user-specific
- your mimeapps.list can contain a reference to some .desktop app that does not exist! 
	- it happened to me that somehow firefox.desktop disappeared
- these are not actual applications but kindof descriptors and pointers to applications, an example:
```
[Desktop Entry]
Name=Firefox
GenericName=Web Browser
Comment=Browse the web
Exec=firefox %u
Icon=firefox
Terminal=false
Type=Application
Categories=Network;WebBrowser;
MimeType=text/html;text/xml;application/xhtml+xml;x-scheme-handler/http;x-scheme-handler/https;
```
- after manually adding such entry to your user apps you must update the mime cache
	- you can see the cache here: `cat ~/.local/share/applications/mimeinfo.cache`
	- `update-desktop-database ~/.local/share/applications` to update it
- after all this:
	- `xdg-mime query default x-scheme-handler/https` will correctly say 'firefox-desktop'
	- firefox will be listed as an app that you can run from apps menu
	- you can do it with any commandline-app! 
- lets do it with obsidian!
	- `which obsidian` get obsidian path
```
[Desktop Entry]
Name=Obsidian
Comment=Markdown notes and knowledge base
Exec=~/Applications/Obsidian-1.9.14.AppImage --no-sandbox %U
Icon=missing
Terminal=false
Type=Application
Categories=Utility;Office;
MimeType=text/markdown;
```

> xdg-open
> will open what you feed it with a default processor for that thingy

### fonts

- `fc-list`
- `fc-cache` 
	- scan font directories to build font cache files
	- i guess you do this when you add fonts manually to:
		- `~/.local/share/fonts` 
	- on ***Arch*** it is recommended to download them through AUR/yay
		- https://wiki.archlinux.org/title/Fonts

### + Integrated Desktop Environment
#### plasma is much more neat
- i used `commonality sol` theme and adjusted its colors to remove the ugly red highlight tone

#### gnome and how it ended

> - **GNOME** is a **Desktop Environment**. It's a complete, cohesive user experience that includes the windows, panels, menus, and system applications. 
> - **GTK** is a **Toolkit**. It's a set of software libraries that developers use to draw the buttons, menus, and text boxes _inside_ those application windows.
> 
> GNOME is fundamentally built _with_ GTK. There is no GNOME without GTK.
> 
> **Here's why this relationship is so fundamental:**
> 1. **Historical Ties**: GTK was originally created _for_ the GIMP image editor (it stands for **G**IMP **T**ool**k**it). The GNOME project later adopted it as their foundation.
> 2. **Deep Integration**: Core GNOME components like: 
> 	   The GNOME Shell (the user interface you see)        
> 	   Nautilus (file manager)        
> 	   GNOME Terminal        
> 	   Settings        
> 	   Text Editor  
> 	   ...are all GTK applications through and through.
> 3. **Co-evolution**: The needs of the GNOME project often drive the development of new features in GTK. When GNOME wants to implement a new user interface paradigm, they frequently add that capability to GTK first.

- `echo $XDG_CURRENT_DESKTOP` in my case it returns GNOME
- `xprop`
	- neat tool that lets you select a window or a thing on desktop and prints interesting properties about it
	- look at `WM_CLASS(STRING)`
- `gnome-session-properties`
	- very useful, here you can select what startup processes go with gnome
	- for example nautilus when i somehow managed to break it
- `nautilus` default file manager for GNOME on ubuntu
- `gnome-tweaks` 
	- for various gnome settings
	- essential for
- `gnome-shell-extension-prefs` 
	- you have to enable here user themes
- put themes in `~/.themes` create if necessary
- put icons in `~/.icons` create if necessary

> and how it ended
```
somehow theme changes nothing about nautilus filesystem

**You've hit a common frustration!** Nautilus (Files) is one of the hardest applications to theme in modern GNOME. Here's why:

## The libadwaita Problem

Since GNOME 42+, Nautilus uses **libadwaita** which:

- **Ignores most GTK themes** by design
    
- **Forces GNOME's default look** (Adwaita)
    
- **Has hardcoded styling** that bypasses theme system
    

## Why GNOME Did This

Officially: "For consistency and developer experience"  
Unofficially: They don't want Nautilus to be themable anymore

goodbye gnome
```


# development

### inotify
> https://unix.stackexchange.com/questions/444998/how-to-set-and-understand-fs-notify-max-user-watches
```
cat /proc/sys/fs/inotify/max_user_watches
```
# audio & video

### PipeWire

> PipeWire is an audio server, similar to others you may have heard of, like Pulseaudio for desktop tasks and JACK for professional audio.
> 
> This means that PipeWire sits between your application and sound drivers (the driver is called ALSA on Linux).
> 
> This is done to make it easier for you to tune as the user by giving you control over each application's volume, as well as making it easier to write applications for it.
> 
> PipeWire is essentially a replacement for the older sound servers, JACK and Pulseaudio, which is more stable and can handle both normal desktop and pro audio use cases, like macOS or Windows can (only better than both imo). It also acts as a video server, giving applications access to your screen when recording or sharing it.
> 
> If you don't do music production there isn't much benefit to go out of your way to install it on a distro which doesn't already have it, but recent versions of Ubuntu and Fedora use it out of the box. Hope this helps!

- `pactl`
	- Control a running PulseAudio sound server. \
	- More information: https://manned.org/pactl.  
	- `pactl info`
		 - Show information about the sound server
- `pw-jack scide`
	- this will run supercollider in a forced jack-mode
	- if all else fails

## video
# tools

## crucial

### awk

> **Best for:** Working with **structured data** (columns, fields, or tabular text).
> Designed as a **small programming language** for text processing.
> Works by splitting lines into fields (by default, whitespace).
> Can perform calculations, conditional logic, and formatted output.


- `awk -flags 'pattern { action }' file`
	- basic sctucture
	- if pattern matched perform action
- `awk '{ print }' file.txt`
	- print entire file
- `awk '{ print $1,$2 }' file.txt`
	- by default awk splits line by any number of whitespaces
	- `$1,$2` prints first and second field `separated by a whitespace`
	- TODO `,` comma seems to be a shorthand for a whitespace
		- `$1" "$2` seems to be equivalent
	- `$0` prints the entire line
	- `NF` prints number of fileds in the line
		- `$NF` can be ised to access the last field
	- `NR` line number
- `awk -F, '{ print $1, $3 }' data.csv`
	- change field separator with flag `-F`

Examples:

> Imagine wanting to just get the file endings.
```
sanctuary/Packages/dev.yarnspinner.unity/SourceGenerator/YarnSpinner.Unity.SourceCodeGenerator.dll
sanctuary/Packages/dev.yarnspinner.unity/SourceGenerator/YarnSpinner.Unity.SourceCodeGenerator.xml
```
`awk -F. '{print "."$NF}' files.txt`
- `-F.` → sets the field separator to `.` (dot)
- `$NF` → refers to the **last field** (everything after the last dot)
- `"."$NF` → adds the dot back to the extension

### sed

> **Best for:** Simple text substitutions and line-based editing.

- `sed -n '1,5p' file.txt`
	- `-n` suppress automatic printing  
	- `'1,5p'` print lines 1 through 5
- `s`
	- the most common use of sed is **substitution**
	- `sed 's/from/replace/g' file.txt`
		- will replace every occurence of word 'from' to 'replace'
		- without the `g` flag it will do it only for every first occurence of pattern per line
- `d`
	- deleting lines
	- `sed '2d' file.txt`
		- delete second line
	- `sed '/^$/d' file.txt`
		- delete empty lines
- `regex`
	- `-E`
		- remember to always use this flag when working with regex in sed
		- otherwise you need to remember which regex operators you need to escape
	- `sed 's/[0-9]/#/g' file.txt`
		- replace any digits with #
		- actually here `-E` is not necessary but it wouldnt harm you anyway
	- `sed -E 's/[0-9]+/*/g' file.txt`
		- but here `-E` is absolutely necessary
		- otherwise you would have to escape + operator
		- this will replace all sequences of numbers with a single asterisk
	- `sed -E 's/cat|dog/pet/g' file.txt`
		- `-E` otherwise you would need to escape `|`
		- this will replace cat or dog words with a word pet
	- `sed 's/^"//; s/"$//' input.txt`
		- using `;` to run two separate `s/` commands
		- together they **strip quotes from both ends** of a line
		- basically they substitute quotation marks with nothing
		- thats how you think about removing things in text by substitution in sed
		- `d` is just for deleting lines


## meow-meow MOVE TO NEAT LINUX TOOLS

>ksnip

- https://github.com/ksnip/ksnip
- neat screenshoter
- had to add `--minimized` to its `.destkop` definition so that it does not open its main window when i login

> lsd

- pretty `ls` replacement with icons and colours!

> ripgrep

- `rg "code\s*\{"` search for "code {" text acrooss all files in the current directory

> starship | zsh better theming

- [backup] starsip config (aka. theme) is at `~/.config/starship.toml`
- make sure to back it up or it will be overwritten when you load a preset
- requires setting it to enabled in `~/.zshrc` with a line `eval "$(starship init zsh)"`
- it is easier to theme starship cos its `.toml` file is way more readable than the dumpsterfire of a config for zsh

> wine

- https://gitlab.winehq.org/wine/wine/-/wikis/Debian-Ubuntu
	- `wget -O - https://dl.winehq.org/wine-builds/winehq.key | sudo gpg --dearmor -o /etc/apt/keyrings/winehq-archive.key -` add repo key
	- check your ubuntu version `cat /etc/os-release`
	- `sudo dpkg --add-architecture i386` add support for installing 32-bit packages on Ubuntu system that is currently running a 64-bit architecture
		- this step is not necessary ubuntu 25.10 onwards
	- `sudo wget -NP /etc/apt/sources.list.d/ https://dl.winehq.org/wine-builds/ubuntu/dists/noble/winehq-noble.sources` add source file
	- `sudo apt update`
	- `sudo apt install --install-recommends winehq-stable`

### web MOVE
- `curl`
	- transfers data from/to a server
	- supports most protocols HTTP(S), FTP, SCP etc.
	- `-f` fail silently instead of printing HTML error page
	- `-s` silent, dont show progress or status messages
	- `-S` show error messages, used commonly when `-s` is used
	- `-L` location, follow redirects of that URL
# cases

### fixing old laptop not connecting to home wifi

- `lspci` see the name of wlan adapter
- `nmcli device status`
- `nmcli connection down "Vodafone-B194"`
	- turn off connection
- `nmcli device disconnect wlp2s0`
	- disconnect wifi device (name from device status)

### dealing with Unity on linux
- `Unity does not save Editor preferences`
	- Apparently Unity tries saving prefs to:
		- `$XDG_CONFIG_HOME/unity3d/Unity/EditorPrefs.asset`
		- Add this to `unityhub.desktop`
		- `Exec=env XDG_CONFIG_HOME=$HOME/.config /opt/unityhub/unityhub %U`
	- Or `-prefs ~/.local/share/unity3d/` to your project editor launch commands in unity hub
- unity crashed
	- `pkill -9 -f "/Editor/Unity"`
	- you might also need to delete `rm -f ~/Projects/sanctuary/sanctuary/sanctuary/Temp/UnityLockfile`

### check if command is part of the #coreutils 

- on ubuntu:
	- `dpkg -S $(which dd)`
- on arch:
	- `pacman -Qo $(which dd)`

### getting count of file types affected by linux clrf thing in my unity project

`cat status.txt | tr -s ' ' | cut -d' ' -f2 | awk -F. '{print "."$NF}'`

### a git tool for diffing whats changed and staging selections
- https://github.com/sharkdp/bat
	- could instead use line:
		- `fzf -m --preview "batcat --color=always --style=numbers --line-range=:500 {}" |`
	- but in this case diff is more useful
- https://pragmaticpineapple.com/four-useful-fzf-tricks-for-your-terminal/

```
# see how --porcelain works
git status --porcelain |
# get only unstaged modified, deleted, renamed or added files
grep '^.[MDRA]' |
# get just the path
cut -c4- |
# remove leading/trailing quotation marks for paths with whitespaces
sed 's/^"//; s/"$//' |
# -m to allow multiple selection with 'tab'
fzf -m --ansi --preview 'git --no-pager diff --color=always -- {} | delta' |
# run git diff on selected files
xargs -r -I{} git add "{}"
```

### corrupted a windows NTFS partition by closing linux os while a file was being copied to that partition

- When you shut down while copying, the NTFS journal was left in an incomplete/dirty state.
- Windows normally repairs this automatically, but **if the partition was last mounted by Linux**, Windows may mark it as hibernated or “in use.”
- Linux will then show errors like:
	- _“The NTFS partition is in an unsafe state”_
	- _“Metadata is corrupted”_
	- _“Cannot read $MFT”_
	- _I/O errors on mount_
- To fix go to windows and run:
	- `chkdsk X: /f`
	- where you replace X with the target disk
- This will:
	- Repair the NTFS journal
	- Fix file system inconsistencies
	- Close open transactions
	- Clear the “dirty” flag
- The output I got:
```
> PS C:\Users\maria> chkdsk D: /f
The type of the file system is NTFS.
Volume label is MEOW.

Stage 1: Examining basic file system structure ...
  5888 file records processed.
File verification completed.
 Phase duration (File record verification): 25.11 milliseconds.
  0 large file records processed.
 Phase duration (Orphan file record recovery): 0.35 milliseconds.
  0 bad file records processed.
 Phase duration (Bad file record checking): 0.29 milliseconds.

Stage 2: Examining file name linkage ...
  10 reparse records processed.
  6594 index entries processed.
Index verification completed.
 Phase duration (Index verification): 44.48 milliseconds.
🔥 CHKDSK is scanning unindexed files for reconnect to their original directory.**
  **1 unindexed files scanned.**
🔥 Recovering orphaned file 2025-11-19_21-53-45.mp4 (664) into directory file 5.**
  **1 unindexed files recovered to original directory.**
🌲 Comment: This means a file was mid-write or not properly added to the directory structure when the shutdown happened.  
🌲 Comment: Windows successfully reattached it. You should now see that file in its original folder.
 Phase duration (Orphan reconnection): 1.30 milliseconds.
  0 unindexed files recovered to lost and found.
 Phase duration (Orphan recovery to lost and found): 0.69 milliseconds.
  10 reparse records processed.
 Phase duration (Reparse point and Object ID verification): 0.75 milliseconds.

Stage 3: Examining security descriptors ...
Security descriptor verification completed.
 Phase duration (Security descriptor verification): 0.75 milliseconds.
  353 data files processed.
 Phase duration (Data attribute verification): 0.29 milliseconds.
🔥 **Correcting errors in the master file table's (MFT) BITMAP attribute.**
🔥 **CHKDSK discovered free space marked as allocated in the volume bitmap.**
🌲 Comment: This is _exactly_ what happens when shutdown occurs while NTFS metadata is being updated.
🌲 Comment: The MFT bitmap tracks which clusters are used/free.  
🌲 Comment: Your partition had inconsistencies (some blocks marked “in use” even though they weren’t).
🌲 Comment: Windows repaired this correctly — Linux **cannot** repair this class of error.

Windows has made corrections to the file system.
No further action is required.

  20447231 KB total disk space.
    426364 KB in 3974 files.
      1420 KB in 355 indexes.
         0 KB in bad sectors.
     36083 KB in use by the system.
     29152 KB occupied by the log file.
  19983364 KB available on disk.

      4096 bytes in each allocation unit.
   5111807 total allocation units on disk.
   4995841 allocation units available on disk.
Total duration: 76.68 milliseconds (76 ms).
```

### too small system partition but most of the space is taken up by snap and flatpak

- Consider making system partition larger than just 40GB next time.
- Stop flatpak and snap services
```
sudo systemctl stop snapd.service
sudo systemctl stop snapd.socket
sudo systemctl stop flatpak-system-helper.service
```
- Move flatpak and snap packages *to where the large partition is mounted, in my case it is /home*
```
sudo mkdir -p /home/.system-storage/snapd
sudo mkdir -p /home/.system-storage/flatpak
sudo mv /var/lib/flatpak/* /home/.system-storage/flatpak/
sudo mv /var/lib/snapd/* /home/.system-storage/snapd/
```
- Mount them back to their original locations:
```
sudo mount --bind /home/.system-storage/snapd /var/lib/snapd
sudo mount --bind /home/.system-storage/flatpak /var/lib/flatpak
```
- Edit **/etc/fstab** file accordingly
```
/home/.system-storage/snapd /var/lib/snapd none bind 0 0
/home/.system-storage/flatpak /var/lib/flatpak none bind 0 0
```

### Error: EMFILE: too many open files, watch ...

- hitting system resource limit when watching files with webpack
- *Basically we had to tell the filesystem to increase the max number of file watchers.*
- `cat /proc/sys/fs/inotify/max_user_watches`