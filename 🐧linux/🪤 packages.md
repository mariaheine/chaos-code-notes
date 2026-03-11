- see #backup for things you should care to back up properly
# crucial

### flatpak
> https://docs.flatpak.org/en/latest/introduction.html

- `install`
	- and target it to `.flatpakref` file
	- `flatpak install flathub org.gimp.GIMP`
		- install from a remote using its remote ID
- `uninstall`
- `remotes`
	- lists remotes
- `remote-add`
	- `flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo`
	- "Here, `flathub` is the local name that is given to the remote. The URL points to the remote’s `.flatpakrepo` file. `--if-not-exists` stops the command from producing an error if the remote already exists."
- `search gimp`
	- search for gimp in all your remotes
- `list`
	- list installed apps
- `update`
	- update installed apps

### fzf
- `ls | fzf | xargs cat` cat the contents of a file selected in fzf
- `find path/to/directory -type f | fzf` start fzf on all files in the specified directory
- `ps aux | fzf`

### tmux

> https://linuxize.com/post/getting-started-with-tmux/

- `tmux`
	- open new session
	- but instead of that you should name sessions:
	- 🌟 `tmux new -s session_name`
		- then to reconnect:
		- `tmux attach-session -t session_name`
- `ctrl+b ?`
	- get list of all key bindings
- `ctrl+b t`
	- return to your normal shell
	- *without killing session*
- `tmux ls`
	- will list active tmux sessions
	- `tmux attach`
		- will attach to the last active session
	- `tmux attach-session -t 2`
		- will attach to the session 
- panes
	- `ctrl+b %` 
		- split horizontally
	- `ctrl+b "`
		- split vertiacally
	- `ctrl+b arrow_keys`
		- navigate panes
	- `ctrl+b ;`
		- toggle between two panes
	- `cb z`
		- toggle pane zoom
	- `cb x`
		- close current pane

# terminal

## alacritty

- ***config***
	- https://alacritty.org/config-alacritty.html
	- likely here: `~/.config/alacritty/alacritty.toml`


## konsole

- `CTRL+SHIFT+M`
	- toggle menu bar
	- useful since otherwise you may want to hide it to have a neat fullscreen terminal

>  my shortcuts
- `ctrl+d`
	- quit session (close view)
- tabs
	- `ctrl+shift+t`
		- new tab
	- `alt+1/2/3/4/5`
		- switch between tabs
- splitting
	- `ctrl+(`
		- vertical split
	- `ctrl+)`
		- horizontal split
- font
	- `ctrl++` enlarge font
	- `ctrl+-` shrink font

## zsh

- `bindkey`
	- will list your key bindings in zsh
	- `bindkey '^K'`
		- will show you what is bound to `CTRL+K`
	- `bindkey -r '^K`
		- will remove the keybinding at `CTRL+K`
		- *you might need that sometimes when things collide with like your Konsole shortcuts or smth*

# cute

### bat

> Colours to everything.
> 
> https://github.com/sharkdp/bat

- `bat --list-themes`
	- you can then set used theme in `~/.zshrc` by adding line:
	- `export BAT_THEME="Catppuccin Mocha"`
- `fzf --preview "bat --color=always --style=numbers --line-range=:500 {}"`
	- good as an alias for searching for a file
- `ya -S bat-extras`
	- this will install `batman`
	- you can then do `batman ssh` instead of `man ssh` and it will be coloured
	- https://github.com/eth-p/bat-extras/blob/master/doc/batman.md

### lsd

> Cute replacement to `ls` with ***neat colours and icons***.

### tree

> Prints super cute tree structures of a given directory.
> 
> Honestly dunno if you even need [[#lsd]] if you have that.

- `-L N`
	- tree depth, default sadly is set to unlimited
- `-C`
	- colourz
- `-a`
	- show hidden

```
❯ tree -L 2 ~ -C  
/home/meowria  
├── bin  
│   └── ssh-manager.sh  
├── thingies  
│   ├── alacritty-theme  
│   ├── comic-shanns  
│   └── matrix  
├── wallies  
│   └── pillarz.png  
└── yay  
   ├── pkg  
   ├── PKGBUILD  
   ├── src  
   ├── yay-12.5.7-1-x86_64.pkg.tar.zst  
   ├── yay-12.5.7.tar.gz  
   └── yay-debug-12.5.7-1-x86_64.pkg.tar.zst
```

# how? what?

### tealdeer

> A Rust implementation of `tldr`
> 
> https://github.com/tealdeer-rs/tealdeer

- `tldr -u`
	- run this first
	- update local cache
	- this can run offline then!!
- `tldr ssh`
	- then run it on any command

### wikiman

>  ***Offline search engine for documentation.*** 
>  
>  Supports manual pages, Arch Wiki, Gentoo Wiki, FreeBSD documentation, and tldr-pages. 
>  
>  More information: https://github.com/filiparag/wikiman#usage.

- `wikiman -S`
	- shows installed wikis
- `yay arch-wiki-docs`
	- install arch wiki
	- OR:
		- `yay -S arch-wiki-lite`
		- for a version without HTML that is better viewable in console

# utility

### ripgrep

> ripgrep is a line-oriented search tool that recursively searches the ***current directory for a regex pattern***. By default, ripgrep will respect gitignore rules and automatically skip hidden files/directories and binary files.
> 
> https://github.com/BurntSushi/ripgrep?tab=readme-ov-file
> 
> GUIDE: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md

- `rg something_you_look_for`
	- will scan for what you are looking for
	- it will not only look for filenames but also within the contents of files!
- `rg something ~/some_path/README.md`
	- will look for `something` in targetted file
### AutoKey

- like autohotkey on windows
- #backup scripts are located in `~/.config/autokey/data/`
- had to add `--daemon` to it's `.desktop` definition so that it does not start with its window open on launch
- Problem: taskbar shows every time i do the `alt+.` shortcut
	- Solution: go to the taskbar settings and in its visibility settings set it to auto hide and allow other programs hide it
# window manager

## i3

***Config***:

>  https://i3wm.org/docs/userguide.html
>  
>  You can find the reference config here `/etc/i3/config`

- `~/.config/i3`
- currently on doomhamster i run something as simple as:
	- umm *simple* ignoring the overcomplicated and dysfunctional setup to have the little matrix sidescreen
```
❯ cat ~/.config/i3/config  
# meow little config  
  
set $mod Mod4  
  
bindsym $mod+Return exec alacritty  
  
bindsym $mod+Shift+q kill  
  
# removing borders  
default_border none  
  
# padding  
#gaps outer 7  
gaps inner 7  
  
bindsym $mod+t split toggle  
bindsym $mod+v split vertical  
  
bindsym $mod+1 workspace 1  
bindsym $mod+2 workspace 2  
bindsym $mod+3 workspace 3  
bindsym $mod+4 workspace 4  
  
#for_window [class=".*"] title_format "t %title c %class i %instance"  
# startup layout  
exec --no-startup-id i3-msg 'workspace 1; append_layout ~/.config/i3/workspace1.json; exec alacritty --class "main";  
exec alacritty --class "matrix" -e bash -c "unimatrix"' && sleep 4 && i3-msg '[class="matrix"] resize shrink left 2  
70; [class="main"] focus'    
  
bar {  
 mode dock  
 status_command i3status  
}
```

