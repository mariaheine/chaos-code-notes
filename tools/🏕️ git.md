🧭🏹 Somehow being back those flags to code in rider

```

//

// ─────────────────────────────────────────────────────────────────────────────── III ──────────

// :::::: E V E N T S T R E A M I N T E R F A C E : : : : : : : :

// ──────────────────────────────────────────────────────────────────────────────────────────────

//

```

# git commands

## authentication

- `git config --global --list | grep credential`
	- if credential helper isnt set to `store` run:
	- `git credential.helper store`
	- this command will be different on windows and macos
- go generate a ***classic token*** with at least `repo` permissions
	- https://github.com/settings/tokens
	- specify expiration time, default is just 30 days
	- *copy the hash you got there*
- `git credential reject`
	- clear previous credentials
	- in the interactive window write following:
		- `protocol=https`
		- `host=github.com`
	- *press Ctrl+D when finished
- then perform any typical git action like cloning one of your private repos
- you will be prompted for your *username and password*
	- use your github username
	- ***don't type the password in!***
	- ***instead paste in the token hash***
- git config will store the authentication then and you will be able to perform your actions
- *if things stopped working again it is likely the token expired*
	- repeat the steps

## curious cat

### status

- `git stauts --porcelain`
- `git status --short`
	- both simply print the status of repo but in a very concise, structured way
	- `--porcelain` doesn't colour the output
	- this is useful when you want to make automated tools
		- like pipe those results to `fzf` and do something with them
- `-uall` 
	- shows ignored files
- `git status filename` 
	- status for a specific file
- want to see `only staged files`?
	- `git diff --cached --name-status`
	- good for an alias
	- `git config --global alias.ss '!git diff --cached --name-status'`
- remember you can pipe long status to **fzf**

### show

- `git show --name-status <commit-hash>`
	- will list all files and their change type affected by that commit
- `git show <commit-hash>:<path-to-file>`
	- will show a file as it was in a certain commit
	- was file deleted in that commit?
		- it won't work
		- with `^` you can easily just mark `first parent of that commit` where that file still existed
		- `git show <commit-hash>^:<path-to-file>`

### `logging`

- `git log <flags/branches> -- path/to/fileOrDirectory`
	- `--` means that everything after this is no longer options or branch names but a path or filename
	- this will restrict history to that specific file or directory
- `-n 5` last 5 commits
- `--all` across all branches 
	- default logs only active branch!
- `commit1..commit2` logs between two commits
- `branch1..branch2` logs commits between two branches (flip branch order in command for the other way around, see aliases for a cool alias that would unite this solution bothways using git functions but it dont work on windows)
- `git log -- filename` history for a specific filename
- `git log --oneline --graph --all --decorate` pretty display, good for an alias
- `git log --stat` which files were changed

- `git log --oneline -- asd/file`
	
	- this will list commits that changed that file

`log -p`
>see changes in each commit
- can be also used on a specific file
	- super userful, lets you see how file changed over commits
	- `git log -p -- .gitattributes`
	- `use relative path` to the repository root (duh)
![[Pasted image 20251109102504.png]]

`log -S'<string>'`
> "Show me commits where the **number of occurrences of this string changed**."

- `-- <path/to/directory>`
	- will only search in that directory
- example
	- i couldn't find the commit where i deleted old zenfulcrum browser proxy, but i knew its more or less location in the project
	- `git log -S'IBrowser' -- sanctuary/Assets/Fillory/scripts/common/proxies`

##  stage actress

- `q` 
	- quit any git active thingy like an endless log or diff
- Git doesn't track empty directories, but it does track directories that contain files.
- Create an empty file like `.gitkeep` inside if you want to force that folder to be trackable

### add

- `git add <file>` 
	- stage a single file
- `git add .` 
	- stage all changes in current directory and subdirectories
- - `git add path/to/dir` 
	- stage all changes (tracked + new files) under myfolder `recursively`
	- so you dont need to add /* at the end
- "what to stage flags"
	- when used without path specification will target all changes
	- `-A` stage all changes in the repo (tracked + untracked + deletions)
	- `-u` only deletions
- `git add --renormalize .`
	- stage all files for renormalization according to .gitattributes
- Instead of writing entire path you can use wildcards:
	- `src/*.js` 
		- all .js files directly in src/ dir
	- `git add **/*.js` 
		- all .js files recursively (requires Bash 4+ or zsh)
	- `**` 
		- is a recursive wildcard to mach (sub)directories at any depth
	- `git add "**/settings/*"` 
		- stage all files whose path contains `/settings/`
		- all files in any 'settings' folder at any depth
		- the `"` are important!
	- `git add **/derp.asset` 
		- will match all `derp.asset` files anywhere in the repo
- `git add -p` will let you interactively stage files and chunks within them

### checkout
> Very multipurpose one.

- `git checkout <path>` can be used to `check out` file under path to the version in last commit

- this can be used to undo deletion of some tracked files!

- and of course to undo all changes to a certain file

- can also target entire directories ex. `**/something/*`

- `git checkout <commit-hash> -- <filepath>`
	- this will bring buck to a working tree a file in the version from the specified commit
	- note, if the file was deleted in that commit:
		- you will get; *error: pathspec 'root/some/path/file.png' did not match any file(s) known to git*
		- you can easily select previous commit to this one with: `<commit-hash>^1`
		- you want this because the previous commit will obvs have that file
	- `--` is a separator that tells git: “Everything after this is a file path, not a branch name or commit.”

- simply `git restore <fielpath>` to undo


### clean

>🔥 USE WITH UTMOST CAUTION
>It might delete things that are not listed when you do git status!`

> 🍰 It is generally used to remove untracked files.

- `git clean -n` 
	- always run this before! 
	- it will show what will be removed
- `git clean`
	- removes **only** untracked files
	- **-f**	untracked files only (force)
	- **-d**	untracked directories too
	- **-x**	untracked and ignored files/directories
	- **-X**	only ignored files/directories
- 🔥  DANGER ZONE
	- `git clean -nd || clean -ndx`
		- always run one of these before below
	- `git clean -fd`
	- `git clean -fdx`
		- will also remove ignored files
	- this broke my project already twice
	- might simply mean you need to regenerate all the files that don't come with a fresh copy of your repo
### commit

- `git commit --amend --no-edit` 
	- add staged changes to the previous commit without editing it's message
	- `git config --global alias.camend 'commit --amend --no-edit'`
- `git commit --amend -m "New commit message"` 
	- rename last commit

> Sanctuary commit emoji naming convention

- 🍰 version release
- ✨ refactoring
- 🧹 cleanups
- 📦 new package / dependency update
- 🛠️ work in progress
- 🌲 milestone / breakthrough
- 🐛 bugfix 

> Aliases for prefixing commits with specific emoji

```
git config --global alias.cfix '!f() { git commit -m "🐛 $*"; }; f'
git config --global alias.crefactor '!f() { git commit -m "✨ $*"; }; f'
git config --global alias.ccleanup '!f() { git commit -m "🧹 $*"; }; f'
git config --global alias.crelease '!f() { git commit -m "🍰 $*"; }; f'
git config --global alias.cpackage '!f() { git commit -m "🗃️ $*"; }; f'
git config --global alias.cwork '!f() { git commit -m "🛠️ $*"; }; f'
git config --global alias.cmilestone '!f() { git commit -m "🌲 $*"; }; f'
```
 


### diff

> Install https://github.com/dandavison/delta for better diffing.

- `git diff` will diff all changes
- `--stat` overwiew of what is changed, summarizing the scale of changes, tells how many lines have been added/removed
- `git diff filename` for a specific file
- `-b` or `-w` ignore whitespace changes
- `--word-diff=color` inline changes
- `--color-moved` different color for moved lines

### remote

> `git remote add origin`
```bash
git init
# Create a repo on GitHub first, then:
git remote add origin https://github.com/yourusername/nvim-config.git
git branch -M main
git push -u origin main
```

### `restore` vs. unstage

- `git restore filename` 
	- revert all changes to a file
- `--staged` 
	- unstage file
	- a bit scary because if you forget this flag will just discard changes to a file
	- thus better make an alias:
	- `git config --global alias.unstage 'restore --staged --'`

## branching

### branch & switch

- `git branch` will list all branches
	- `-r` remote ones
	- `-a` both local and remote
	- `-vv` additional info
	- `-d` safely delete a local branch
- `git branch <name>`
- `git switch <name>`
- `git switch -c <name>` single step create branch and switch

### merge

> 🧞‍♀️ Remember you always merge `INTO` the branch you are currently on!

1. `git switch <target_branch>` 
	- switch to the target branch you want to merge changes into
2. `git pull origin <target_branch>` 
	- make sure it is up to date
3. `git merge <source_branch>` 
	- to merge the changes

## among others

### pull
> The merge/rebase of pull always targets your current HEAD.

- `git pull`
	- by default uses `merge`
	- it is basically a shorthand for:
		- `git fetch`
			- so it fetches all remote branches, because that is how fetch works
		- `git merge origin/<current-branch>`
			- but it only merges in the current branch
	- this behavior can be changed by the config line, which defaults to:
		- `pull.rebase=false`
		- after changing this to true rebase will be used by default
- `git pull origin woof`
	- if you do this while on branch `meow`
	- it will fetch and merge into your HEAD (on `meow`  naturally) the branch `woof`
- `git pull --rebase`
	- will use rebase instead of default merge
- `-X` flag
	- for both merge and rebase you can add this flag to specify which sife of the equation will be used to overwrite changes
	- beware it means opposite for merge and rebase

> The confusing table of what ours/theirs mean respectively for merge/rebase. In short, merge means what you would expect, rebase what you wouldn't

| Operation  | `-X ours` keeps | `-X theirs` keeps |
| ---------- | --------------- | ----------------- |
| **merge**  | local changes   | remote changes    |
| **rebase** | remote changes  | local changes     |

### push

### set-upstage

## imtermediate

### `git submodule`

- `git submodule add https://repository_clone_path.git relative/Path/to/.git` adding a submodule
- `git submodule update --remote --merge` update the submodule in your main project
- `git submodule update --init --recursive` init all submodules
- force re-fetch: `git submodule foreach --recursive 'git fetch --all && git checkout main || git checkout master'`

### rm
> deletes file from Working Directory (actual filesystem) and Staging Area
> https://stackoverflow.com/questions/2047465/how-do-i-delete-a-file-from-a-git-repository

- **--cached**
	- `git rm --cached file1.txt` delete file from index but keep the file
	- `git rm -r --cached myDirectoryName` delete directory from index but without deleting the files
		- 🔥 `after this you need to commit this deletion action`

### `git ls-files`

  

> Lists files tracked by git, used to verify if file is tracked or to verify which files are ignored

  

- `git ls-files directory-name/` do it but only for a specific directory

- `--others` shows untracked files.

- `--ignored` lists ignored files (based on .gitignore).

- `--exclude-standard` applies .gitignore rules when used with other options.

- `git ls-files --others --exclude-standard` show untracked files but not the ignored ones

- `git ls-files -- path/to/your/file.txt`

- check if file is tracked by git

- if the otuput is empty it is not tracked

- `git check-ignore -v path/to/your/file.txt` check if it is ignored

  

### `cherry-pick`

  

### 🏛️ lfs

- `git lfs prune` This will remove LFS files that are no longer referenced by any commits in your repository. It's a good way to clear out unnecessary files, but be cautious, as this will also remove files that are no longer needed (but not yet cleaned up in your repository's history).

  



  

### `tag`s and releases

- `git tag -a v1.0.0 -m "Tag name"` create a tag
- `-a` stands for annotated, create an annotated tag named v1.0.0
- `-m` attach a message to the tag, required
- `git push origin v1.0.0` tags need to be explicitly pushed!
- `git push` alone will not work
- `git push --tags` to push all created locally tags at once, rare case i guess

## 🐛 debugging

1. `git checkout <good-commit> -b <debug_branch_name>`

  

# 🗻 projetct setting up

### 1. .gitignore

- see [[#rm]] section on how to stop tracking files that are already in history
	- of course you also need to add them to .gitignore
	- and then remove them from cache
	- and commit the deletion of these cached files
	- this is NOT rewriting history
- `git check-ignore -v <filepath>` if some file is missing after fresh cloning a repo it will be likely ignored, this command will show you which entry (line) in your gitignore is causing this

### 2.  `.gitattributes`

> Setting this up is equally important to setting up `.gitignore`, you will avoid many headaches if you do that at the very beginning. Especially if you plan on using `lfs` or making this project worked on cross-platform.
> 
> https://git-scm.com/docs/gitattributes

- `git add --renormalize .`
	- you need to run it if you make changes to or add `.gitattributes` to an existing project
- `text`
	- use this to mark all text files in your repo
	- `eol`
		- allows you to specify if it is not specified that files line endings in the working directory are determined by the `core.autocrlf` or `core.eol` configuration variable
		- can be set to `crlf` (don't do it) or `lf`
			- just mark all your text files to `lf` and you will be more relaxed
			- to my understanding `crlf` only makes sense on some windows legacy projects
	- `text=auto`
		- lets git guess whether that file is binary or text
- `-text`
	- i kid you not `this means to interpret file as binary`
	- could this be more confusing
	- the logic for `-` sign is to `disable the text processing` for that type of files
- `merge`
	- select the merge tool for that type of file
	- this is common in unity
		- `*.asset merge=unityyamlmerge text=auto`
		- `*.controller merge=unityyamlmerge eol=lf`
	- but also for `lfs`
		- `*.dll filter=lfs diff=lfs merge=lfs -text`
- `diff`
	- TODO
- `filter`
	- TODO

> There is a neat Unity `.gitattributes` example file here: https://gist.github.com/nemotoo/b8a1c3a0f1225bb9231979f389fd4f3f
> 
> But the problem with it is that it doesnt mark line-endings style for text files.:

```
|*.cs diff=csharp text|
|*.cginc text|
|*.shader text|
```

improved
```
# ---------- Unity Text Files ----------
*.cs text eol=lf
*.cginc text eol=lf
*.shader text eol=lf
*.asmdef text eol=lf
*.json text eol=lf
*.txt text eol=lf
*.md text eol=lf
*.xml text eol=lf
```

### CRLF vs. LF and .gitattributes

- `git config --global core.autocrlf input`
	- https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration#_core_autocrlf
	- `input` converts CRLF → LF on commit, leaves working copy as-is.
- `git config --global core.safecrlf true`
	- prevents accidental commits with mixed line endings
#### Converting a windows project from CLRF to LF line endings for cross-system compatibility AND how to avoid this problem in the first place. 

> Because CRLF if kindof useless by now, it is still used as default on windows for old stuff compatibility, which could only make sense in really specific situations unbeknown to me.

- Add proper `.gitattributes`, below a basic example for a unity subpackage, marking files for either LF or LFS if they are binary.
	- For a proper lfs coverage this is a nice one aswell: https://gist.github.com/nemotoo/b8a1c3a0f1225bb9231979f389fd4f3f
	- But you need the Unity Text Files explicit handling for LF file endings.
	- Make this a habit whenever starting a new project.
	- Any user who clones the repo and commits files will follow these rules.
	- `git add --renormalize .`
```
# ---------- Unity Text Files ----------
*.cs text eol=lf
*.cginc text eol=lf
*.shader text eol=lf
*.asmdef text eol=lf
*.json text eol=lf
*.txt text eol=lf
*.md text eol=lf
*.xml text eol=lf

# ---------- Unity YAML Assets (LF) ----------
*.prefab merge=unityyamlmerge eol=lf
*.unity merge=unityyamlmerge eol=lf
*.controller merge=unityyamlmerge eol=lf
*.anim merge=unityyamlmerge eol=lf
*.mat merge=unityyamlmerge eol=lf
*.physicMaterial merge=unityyamlmerge eol=lf
*.physicsMaterial2D merge=unityyamlmerge eol=lf
*.meta merge=unityyamlmerge eol=lf
*.asset merge=unityyamlmerge text=auto

# ---------- Binary / Large Assets (Git LFS recommended) ----------
*.png filter=lfs diff=lfs merge=lfs -text
*.jpg filter=lfs diff=lfs merge=lfs -text
*.jpeg filter=lfs diff=lfs merge=lfs -text
*.gif filter=lfs diff=lfs merge=lfs -text
*.tif filter=lfs diff=lfs merge=lfs -text
*.mp3 filter=lfs diff=lfs merge=lfs -text
*.wav filter=lfs diff=lfs merge=lfs -text
*.fbx filter=lfs diff=lfs merge=lfs -text
*.dll filter=lfs diff=lfs merge=lfs -text
*.unitypackage filter=lfs diff=lfs merge=lfs -text

# ---------- Ignore auto-generated directories ----------
/Library/
/Temp/
/Obj/
/Build/
```
- ALL BELOW ARE OPTIONAL
- Optional “dos2unix” step
	- Running `dos2unix` locally just converts your **working copy on disk** to LF for comfort.
	- It does **not affect what Git stores**.
	- Any new clone or checkout would already give LF because of `.gitattributes`.
- Set your config properly
	- `git config --global core.autocrlf input
	- `git config --global core.safecrlf true`
- For an old windows project we will now renormalize existing text files
	- `file Runtime/rzeka/interface/IRzeka.cs` 
		- this will return:
		- `Runtime/rzeka/interface/IRzeka.cs: C++ source, ASCII text, with CRLF line terminators`
	- since the project was created on windows without proper .gitattributes we can see CRLF endings there
- To renormalize all text files in the project to LF we will then proceed to use `dos2unix`
```
# Unity scripts
find . -type f -name "*.cs" -exec dos2unix {} +

# Unity text assets
find . -type f \( -name "*.json" -o -name "*.asmdef" -o -name "*.txt" -o -name "*.md" -o -name "*.xml" \) -exec dos2unix {} +

# Unity YAML files
find . -type f \( -name "*.prefab" -o -name "*.unity" -o -name "*.controller" -o -name "*.anim" -o -name "*.mat" -o -name "*.physicMaterial" -o -name "*.physicsMaterial2D" -o -name "*.meta" -o -name "*.asset" \) -exec dos2unix {} +
```
- After running this
	- `file Runtime/rzeka/interface/IRzeka.cs` will no longer find CRLF line endings
		- `Runtime/rzeka/interface/IRzeka.cs: C++ source, ASCII text`
	- you will have a lot of files to commit

# 🤷🏻‍♀️🪵 git config

### basics
> IMPORTANT. `git config` stuff is not tracked anywhere! These are your local workspace setttings.

- `git config --list`
	- view all your Git config (includes aliases)
- `git config --global --list`
	- but only for global configs
- [backup] `~/.gitconfig`
	- thats where it is stored

### cool aliases

- `git config --global alias.s "status"`
	- see [[#status]] for status aliases
- `git config --global alias.c "commit -m"`
	- see [[#commit]] for commit aliases
- `git config --global alias.unstage "restore --staged"` 
	- safer way to unstage a file than using restore manually (can clear file changes accidentaly)
- `git config --global alias.plog "log --oneline --graph --decorate"` 
	- decorated log of `branch1..branch2` or `--all` or something

While you remove an alias with: `git config --global --unset alias.<alias-name>`

# Git Types

  

### Tree Entry Target Types

  

- Blob (just contents of the file, no metadata)

- Tree (a directory listing of blobs an trees)

- Git Link

  

### Areas

  

- Commit (`snapshot of the working tree`)

- Working Dir (`.git working directory`)

- Repository (`collection of commits, branches, HEAD etc.`)

- Index / Staging Area (`playground for the next commit`)

- *git does NOT directly commit changes from the working dir to the repository!*

- changes are first registered into the index

  
  

### Branches

  

`Branch is just a pointer to SHA (named reference) to a specific commit` 🔥

  

This means when you commit some new changes in a new branch the branch will change to point to that new commit!

  

- `git branch` just creates a pointer to the latest commit in a branch we are currently on

- `HEAD` stores the reference to the current branch

- it points to a branch

- which in turn points to a commit

- `git checkout <branch>` tells the `HEAD` to point to a specified `branch`

- `git checkout -b <branch>` will create a `new branch` and point `HEAD` to it

- `git checkout <good-commit> -b <branch>` create a branch from specific commit, good for debugging

  

### Working with the

  

- new file in Working Dir is not automatically `tracked`

- git status will then say

  

### Repository from scratch (`without git init`)

  

- create `.git` directory

- git is always:

- a collection of objects

- `mkdir .git/objects`

- a system for naming those objects

- `mkdir .git/refs`

- `mkdir .git/refs/heads` (references to branches)

- `echo ref: refs/heads/main > .git/HEAD` (reference to the head branch)

- 🔥 already here git status will work

- even though we don't even have branch `main` yet, we told it there is one