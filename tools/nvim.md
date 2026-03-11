
# installation

> Afterall the easiest and most dependable way of installing nvim is through an AppImage available at their releases github page here:
> 
> https://github.com/neovim/neovim/releases
> 
> Using apt gets and outdated version, flatpak creates a strict sandbox that causes messy difficulties with plugins.

# config

> Located at `~/.config/nvim/init.lua`

- [ ] there is also stuff at `~/.local/share/nvim/`
	- [ ] but i am not sure what really goes there
- 
	- probs add it to your
- `lsd -la ~/.local/share/nvim/site/autoload`
	- i guess auto loaded plugins on nvim startup are here?

# basics

## 🦜 normal mode

`Escape` get out of insert mode

- It is a common thing to remap `Caps Lock` to `Escape` because it is much easier to reach and not such an useful key.

### 🛠️ undo/redo

- `u`
	- undo
- `ctrl + r`
	- redo

### 📋 clipboard

> https://stackoverflow.com/questions/11489428/how-can-i-make-vim-paste-from-and-copy-to-the-systems-clipboard

- check if your vim supports clipboard
	- `echo has('clipboard')`
	- if it returns `0` then you need to install like `vim-gtk3`

### 🔍 finding stuff

#### within a given line

- `f/F` finding stuff ***within*** a given line

	- `fo` finds next 'o'
	- `Fo` finds previous 'o'
	- ✨ tip: `f)`
		- will often be useful to jump to the closing function nawias

- `t/T`

	- like `f` but stops before the given character
	- ✨ tip: `vt(` -> will select untill the opening bracket
	- ✨ tip: `ct(` -> will delete text untill `(` character and start insert

- ***for both***:
	- `;` repeat last find motion
	- `,` repeat last find motion in opposite direction

#### globally

- ✨ `/ & ?`

	- `/sometext` search for  the next *sometext* string occurence
	- `/;` 🏴‍☠️ is a useful one in C#
	- `?` search *backwards* in file
	- for both `n/N` next/*opposite direction* occurence of the last search

### 🧭 movement

#### basics

- `j/k` down/up
- `h/l` left/right
- `5j` 5 lines down
- ✨ `$` end of the line
- ✨ `_` first ***non blanc*** character of the line
- ✨ `''` move to the previous location before a 'jump'
#### page scrolling

- ✨ `{ or }` 
	- paragraphs behind/forward
	- will grab the whitespaces between paragraphs
- ✨ `( or )` *sentences* behind/forward
	- this will be slower than { or }
	- but will let you grab first character of a new paragraph
- `ctrl + D/U`
	- like a big leap up and down
	- dunno how it works precisely

#### word jumping

- ✨ below are actually WAY more useful than you would think initially
- often they will be easier and faster to reach than line ending, beginning
- don't forget to use the capital versions in dense code

- `w/W` next word beginning
- `e/E` this/next word ending
- `b/B` move back a word
#### gg / G

- `gg/G` first/last line of the file
- `:4` go to line 4
	- another variant of this is `4G`
- but using `:` is probably nicer because you can see what numbers you write

#### HML
  
- `H` go to the top of the screen
- `M` go to the middle of the screen
- `L` go to the bottom of the screen

#### movement more (useless? too situational?)

- `%` jump to twin parenthesis to the one under caret
- 🪞 tip: `v%` -> probably most useful for selecting everything between those parentheses and then likely deleting it
- 🦫 `*/#` next/previous occurence of the word under caret

###  🌬️ windows and tabs

> Remapped in `init.lua` to:
```lua
-- Window navigation
vim.keymap.set('n', '<C-h>', '<C-w>h', { desc = 'Move left' })
vim.keymap.set('n', '<C-j>', '<C-w>j', { desc = 'Move down' })
vim.keymap.set('n', '<C-k>', '<C-w>k', { desc = 'Move up' })
vim.keymap.set('n', '<C-l>', '<C-w>l', { desc = 'Move right' })

-- Tab navigation
vim.keymap.set(
	'n', '<leader>tn', ':tabnext<CR>', { desc = 'Next tab' })
vim.keymap.set(
	'n', '<leader>tp', ':tabprev<CR>', { desc = 'Previous tab' })
vim.keymap.set(
	'n', '<leader>tf', ':tabnew<CR>', { desc = 'New tab' })
vim.keymap.set(
	'n', '<leader>tc', ':tabclose<CR>', { desc = 'Close tab' })
```

> windows
```lua
<C-w>h     " Move left
<C-w>j     " Move down
<C-w>k     " Move up
<C-w>l     " Move right
<C-w>w     " Cycle through windows

<C-w>c     " Close current window/split
:close     " Same as above
<C-w>q     " Close current window (quit if last window)
:q         " Close current window

-- you won't be likely directly using below
:vs        " Vertical split
:vsplit    " Same as above
<C-w>v     " Vertical split

:sp        " Horizontal split
:split     " Same as above
<C-w>s     " Horizontal split
```

### 🗑️ deletion

---

#### x

- `x` delete character *under* the caret
- `X` delete character *before* the caret

#### d

- `d` delete content (and copy it to clipboard!)
	- `dw` delete to next word beginning
	- `dt(` delete to the `(` character
	- `d2w` delete next two words
- `D` delete to the end of the line
- `dd` delete line
	- `3dd` deleted three lines

#### s

- `s` -> delete character and start insert
- 🦜 `S` -> delete line and start insert

#### c

- `c` -> delete *selection* and start insert
- 🦜 `cw` -> delete entire word and enter insert mode
- 🦜 `C` -> delete from the cursor to the end of the line and start insert
	- note: same as `D` but starts insert

#### r

- `r` replace single character under caret without switching to insert mode

### 📖 setting marks

- `mA` sets a *global* mark 'A' to the current line
- `'A` jump back to the A-marked line
- `F3` remove mark from the given line

Additional marking stuff:

- `:marks` lists all marks
- `:delmarks A` deletes all marks 'A'
### 🦫 random things
#### magic tricks

- ✨ `zz` -> center screen to the cursor position
- `.` repeat previous command
- ✨ `J` -> join line
#### capitalization / big letters

- `gUl` capitalize single letter under the cursor and move to the left
- `gUw` captialize word
- `gU$` capitalize to the end of the line
- `gUU` capitalize entire line

#### auto `json` formatting

- `:%!jq .`
	- Roughly, this is entering a command, `:`, on the whole buffer, `%`, then executing a shell command, `!`, of `jq .` the most basic `jq` invocation. Presumably, you could entry and `jq` filter command to form some other JSON instead of just formatting the given JSON, but I’ll leave that up to you to play with.
- https://codegoalie.com/posts/format-json-nvim-jq/

#### indentation

- `>> or <<` 
	- (de)indent current line in normal mode
- `> or <`
	- same but for visual mode
	- this is how you do it neatly for multiple lines at once
### 🍰 copy & paste

- `y` yank stuff
- examples:
	- `yw` copy word
	- `yk` copy line above
	- `Y` 🏴‍☠️ yank (copy) entire line
- `p/P` paste below/above cursor

- ✨🌌 important spell: `"0p` 
	- paste last yanked thing even after some deletion

## 🏹 insert mode

### i/a

- `i/a` -> enter insert mode before/after caret
- 🦜 `I/A` -> enter insert mode at the beginning/end of the line
- `10i-Esc` -> will enter insert mode with 10x duplication of - character on Esc

### o

- `o/O` -> open new line below/abowa
## 🪞 visual mode

- `v` 
	- enter visual mode, it allows you to select stuff with movement keys and then do what you want with that selection
	- ie. `d` to delete it
- 🔥 `o` -> switch selecting direction
- 🔥 `u/U` make all selected things lower case/upper case

### visual block mode

- `ctrl+v`
	- enter `Visual Block Mode`
- move up/down *to select multiple lines*
- then choose:
	- `shift+i` 
		- to insert text before the block
		- notice you will see it initially inserting only on the first line of your selection block
		- type text
	- `shift+a`
		- insert text at the end of selected lines
	- `d`
		- delete a column of selected lines
- `Esc`

# plugins

### TODO

- https://github.com/folke/noice.nvim
- https://github.com/lewis6991/gitsigns.nvim
- https://github.com/folke/flash.nvim
- https://github.com/sharkdp/fd

### comments

> https://github.com/numToStr/Comment.nvim

- normal mode:
	- `gcc`
		- toggle line comment
- visual mode:
	- `gc` all lines line comment
	- `gb` all lines block comment
### lazy.nvim

> Plugin manager
> 
> git: https://github.com/folke/lazy.nvim
> docs, installation: https://lazy.folke.io/installation

- plugin manager for nvim that works with `.lua` configs
- add plugins by:
	- create a `.lua` file at `~/.config/nvim/lua/plugins`
	- if you don't have that dir it means you didn't install lazy.vim properly
	- name that file whatever, use them to group your relakffted plugins to keep it nice and clean
	- *remember you need to wrap plugins in*:
```lua
return {
	-- ... plugin installation here
}
```

### lsp

- `ctrl ]`
	- go to the definition
	- `ctrl t`
		- go back
- `grr`
	- see references to what you are currently over
- `grn`
	- rename!!
- `=G`
	- format
- `K`
	- capital `K` over some thing will show neat type info about it
### telescope

> This has SO MANY configuration options.
> 
> See plugin `.lua` config for keymappings.

- `enter`
	- open in current window
- `ctrl t`
	- open in new tab
- `ctrl v`
	- open in new vertical split

### tree

> Neat visual tree of the files
> 
> ***Check `.lua` config in plugins to see additional shortcuts in case u forgot them.***

- `g?`
	- show bindings
- `a`
	- create a file or directory
	- *to create a directory you must put a trailing `/`*
- `u`
	- rename full path
	- *this can be used to move files*
- `x & p`
	- cut and paste file
- `ctrl t`
	- open file in new tab
- `ctrl v`
	- open file in new vertical split

### which-key

> https://github.com/folke/which-key.nvim