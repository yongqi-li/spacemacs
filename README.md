# Spacemacs

_The best editor is not *Emacs* nor *Vim*, the best editor is
*Emacs+Vim* !_

**Table of Contents**  *generated with [DocToc](http://doctoc.herokuapp.com/)*

- [Goals](#user-content-goals)
- [Screenshot](#user-content-screenshot)
- [Who can benefit from this ?](#user-content-who-can-benefit-from-this-)
- [Install](#user-content-install)
- [Packages and Extensions Organization](#user-content-packages-and-extensions-organization)
- [How it works](#user-content-how-it-works)
- [How to switch to normal mode efficiently ?](#user-content-how-to-switch-to-normal-mode-efficiently-)
- [Evil plugins](#user-content-evil-plugins)
- [How to use it ?](#user-content-how-to-use-it-)
	- [Executing Vim and Emacs commands](#user-content-executing-vim-and-emacs-commands)
	- [Key bindings help](#user-content-key-bindings-help)
	- [About helm](#user-content-about-helm)
	- [Navigation](#user-content-navigation)
		- [Point/Cursor](#user-content-pointcursor)
		- [ace-jump mode](#user-content-ace-jump-mode)
		- [Buffers and Files](#user-content-buffers-and-files)
		- [Bookmarks](#user-content-bookmarks)
	- [Window manipulation](#user-content-window-manipulation)
	- [Text manipulation commands](#user-content-text-manipulation-commands)
	- [Spell checking](#user-content-spell-checking)
	- [Region selection](#user-content-region-selection)
	- [Region narrowing](#user-content-region-narrowing)
	- [Auto highlight](#user-content-auto-highlight)
	- [Color theme](#user-content-color-theme)
	- [UI elements](#user-content-ui-elements)
        - [Mode-line](#user-content-mode-line)
        - [Toggles](#user-content-toggles)
	- [Minor Modes](#user-content-minor-modes)
	- [Line formatting](#user-content-line-formatting)
	- [Errors handling](#user-content-errors-handling)
	- [Project management](#user-content-project-management)
	- [Modes](#user-content-modes)
		- [Helm](#user-content-helm)
		- [Erlang](#user-content-erlang)
		- [Ledger](#user-content-ledger)
		- [Lisp](#user-content-lisp)
		- [Magit](#user-content-magit)
		- [Org](#user-content-org)
		- [Perforce](#user-content-perforce)
		- [Python](#user-content-python)
			- [Inferior REPL process](#user-content-inferior-repl-process)
			- [Testing in Python](#user-content-testing-in-python)
			- [Other Python commands](#user-content-other-python-commands)
		- [R (ESS)](#user-content-r-ess)
			- [Inferior REPL process](#user-content-inferior-repl-process-1)
			- [Other R commands](#user-content-other-r-commands)
		- [rcirc](#user-content-rcirc)
	- [And more...](#user-content-and-more)
- [TODO list](#user-content-todo-list)

## Goals

- The main goal of `Spacemacs` is to bring *the power of Vim modal editing* to
*the powerful Emacs editing platform*.

- The ultimate goal for `Spacemacs` is to pack as much stuff as possible while
keeping a "blazingly fast" boot time. Currently `Spacemacs` ships with more
than 120 packages and its load time is approx. 2 seconds on my Macbook Air
2014.

- Slick integration with Evil states/Vi modes, tries as much as possible to
keep your fingers on the home row, no matter the mode you are in.

- Minimalistic UI, keep your available screen space for what matters: your
text files.

## Screenshot

![screenshot](https://raw.githubusercontent.com/syl20bnr/spacemacs/master/doc/spacemacs.png)

## Who can benefit from this ?

`Spacemacs` is first intended to be used by **Vim users** who want to go to the
next level by using Emacs.

It is also a good fit for people wanting to **lower the [risk of RSI][RSI]**
induced by the default Emacs key bindings. 

People wanting to learn **a different way to edit files** or wanting to learn
Vim key bindings.

As a note side, if you are a programmer and you don't know Vim key bindings
yet, I deeply recommend you to learn the basics as recommended in
[Sacha Chua's one-page guide][sacha_guide] about how to learn Emacs.

## Install

`Spacemacs` is only tested with `Emacs 24.3.1`.

    cd ~
    mv .emacs.d .emacs.bak
    git clone http://github.com/syl20bnr/spacemacs .emacs.d
    cd .emacs.d
    mkdir .desktop

Get the sub-modules:

    cd ~/.emacs.d
    git submodule init
    git submodule update

## Packages and Extensions Organization

Modes/libraries are separated into two categories:
- **package:** packages installed from `package.el` compliant repositories.
- **extension:** modes directly fetched from git repositories (as sub-modules)

There are two types of extensions:
- **pre-extensions:** loaded before packages
- **post-extensions:** loaded after packages

Modes/libraries are listed in the following three files depending whether they
are packages, pre-extensions or post-extensions, in order of loading:
- pre-extensions.el 
- packages.el
- post-extensions.el

A package or extension may have a corresponding `init-....el` file responsible
for its initialization, those files are stored respectively in the
`init-package` and `init-extension` directories.

`init-....el` files lazy load the modes by using the [use-package][use-package]
macro.

## How it works

`Spacemacs` uses the [evil][evil] mode to emulate Vim key bindings. It is a
very complete emulation (the most complete I've seen yet).

`Spacemacs` heavily uses the [evil-leader][evil-leader] mode which brings the
Vim leader key to the Emacs world.

This leader key is commonly set to `,` by Vim users, in `Spacemacs` the leader
key is set on `SPC` (space bar, this is why the name `spacemacs`). This key is
the most accessible key on a keyboard and it is pressed with the thumb which is
a good choice to lower the risk of [RSI][RSI].

So with `Spacemacs` there is no need to remap your keyboard modifiers to
attempt to reduce the risk of RSI, every command can be executed very easily
while you are in `normal` mode by pressing the `SPC` leader key, here are a
few examples:

- Save a buffer: `<SPC> f s`
- Save all opened buffers: `<SPC> f S`
- Open (switch) to a buffer with `helm`: `<SPC> b s`

## How to switch to normal mode efficiently ?

One of the main design flaw in Vim key bindings is that you often have to press
the `ESC` key to return to `normal` mode and `ESC` key is very far from the
home row.

The popular way to avoid this is to replace `ESC` by `jj` pressed rapidly. In
`Spacemacs` you have to press `fd` quickly by default.

`fd` also works to quit minibuffer prompts.

**Note:** For those who know about [keychords.el][keychords] mode. This mode is
not used to achieve this, the reasons for this is latency and the fact that
keychords wants you to press several keys almost at the same time which is
something very difficult to master correctly on a keyboard. `Spacemacs` has a
special function called `fd-trigger` to handle the `fd` key sequence and fix
the above keychords issues.

## Evil plugins

`Spacemacs` ships with the following evil plugins:

                 Mode                   |                          Description
----------------------------------------|--------------------------------------
[evil-leader][evil-leader]              | vim leader that bring a new layer of keys in normal mode
[evil-little-word][evil-plugin01]       | port of [camelcasemotion.vim][vim-plugin01]
[evil-operator-comment][evil-plugin01]  | comment/uncomment with `CC`
[evil-matchit][evil-plugin02]           | port of [matchit.vim][vim-plugin02]
[evil-visualstar][evil-plugin03]        | search for current selection with `*`
[evil-exchange][evil-plugin05]          | port of [vim-exchange][vim-plugin04]
[surround][evil-plugin04]               | port of [surround.vim][vim-plugin03]

## How to use it ?

Every sequences must be performed in `normal` mode.

### Executing Vim and Emacs commands

Vim commands are execute as usual with the `:` key.

To execute an Emacs command:

    <SPC> :

### Key bindings help

A list of all the key bindings can be accessed by pressing:

    <SPC> ?

To narrow the list to `Spacemacs` specific key bindings set the pattern to `SPC`

### About helm

`Spacemacs` tries to use [helm][helm] as much as possible.
[helm][helm] is coupled to [popwin][popwin] so `helm` window always appears in
a new temporary windows at the bottom.

The following `helm` modes are installed with `Spacemacs`:

Mode                                    | Key Binding | Description
----------------------------------------|-------------|------------------------
[helm-swoop][hswoop]                    | `<SPC> h s` | search for occurrences within a file and edit the result
[helm-css-scss][hcss]                   | `<SPC> h c` | for quick navigation in CSS
[helm-c-yasnippet][hyas]                | `<SPC> a y` | select snippets
[helm-themes][hthemes]                  | `<SPC> h t` | select a theme
[helm-projectile][projectile]           | `<SPC> p f` | select files within a projectile project
[helm-descbinds][hdescbinds]            | `<SPC> ?`   | show key bindings
[cofi/helm-flyspell-correct][hflyspell] | `<SPC> s c` | choose a corrected word

### Navigation

#### Point/Cursor

Navigation is performed using the Vi key bindings `hjkl`.

Key Binding |                 Description
------------|------------------------------------------------------------------
`h`         | move cursor left
`j`         | move cursor down
`k`         | move cursor up
`l`         | move cursor right
`H`         | move quickly up (10 lines at a time)
`L`         | move quickly down (10 lines at a time)
`<SPC> j h` | go to the beginning of line (and set a mark at the previous location in the line)
`<SPC> j l` | go to the end of line (and set a mark at the previous location in the line)
`<SPC> z z` | lock the cursor at the center of the screen 

#### [ace-jump mode][ace-jump]

Key Binding |                 Description
------------|------------------------------------------------------------------
`,`         | initiate ace jump
`<SPC> ,`   | go back to the previous location (before the jump)

#### Buffers and Files

`Spacemacs` uses `ido` for opening files since `ido` way to navigate
the file system is better than `helm` in my opinion (especially because `ido` can
remember the last selected directories and buffers, maybe helm can do this ?).
`ido` is also used to kill buffers.

Buffer manipulation commands (start with `b`):

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> b d`   | delete the current buffer (beware the associated file is also deleted)
`<SPC> b k`   | kill the current buffer
`<SPC> b K`   | kill all buffers except the current one
`<SPC> b m h` | move a buffer to the left
`<SPC> b m j` | move a buffer to the bottom
`<SPC> b m k` | move a buffer to the top
`<SPC> b m l` | move a buffer to the right
`<SPC> b n`   | switch to next buffer
`<SPC> b p`   | switch to previous buffer
`<SPC> b r`   | rename the current buffer
`<SPC> b s`   | switch to a buffer using `helm`
`<SPC> b w`   | toggle read-only

Files manipulation commands (start with `f`):

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> f f`   | open a file using `ido`
`<SPC> f i`   | open your `init.el` file
`<SPC> f s`   | save a file
`<SPC> f S`   | save all files
`<SPC> f y`   | show current file absolute path in the mini buffer

#### Bookmarks

Bookmarks can be set anywhere in a file. Bookmarks are persistent. They are very
useful to jump to/open a known project.

Bookmarks commands (start with `k`):

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> k d`   | delete a bookmark
`<SPC> k g`   | go to a bookmark using `helm`
`<SPC> k r`   | rename a bookmark
`<SPC> k s`   | save a bookmark

### Window manipulation

Split windows are dynamically resized depending on whether they are selected or
not. Resizing is performed by the [golden-ratio][golden-ratio] mode.

Every window has a number displayed at the start of the mode-line and can
be quickly accessed using `<SPC> number`.

Key Binding   |                    Description
--------------|----------------------------------------------------------------
`<SPC> 1`     | go to first window
`<SPC> 2`     | go to window number 2
`<SPC> 3`     | go to window number 3
`<SPC> 4`     | go to window number 4
`<SPC> 5`     | go to window number 5
`<SPC> 6`     | go to window number 6
`<SPC> 7`     | go to window number 7
`<SPC> 8`     | go to window number 8
`<SPC> 9`     | go to window number 9
`<SPC> 0`     | go to window number 10

Windows manipulation commands (start with `w`):

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> w b`   | split a window horizontally
`<SPC> w c`   | close a window
`<SPC> w d`   | toggle window dedication (dedicated window cannot be used by a mode)
`<SPC> w H`   | move window to the left
`<SPC> w J`   | move window to the bottom
`<SPC> w K`   | move window to the top
`<SPC> w L`   | move window to the right
`<SPC> w m`   | maximize/minimize a window
`<SPC> w M`   | maximize/minimize a window, when maximized the buffer is centered
`<SPC> w p`   | close the current sticky popup window
`<SPC> w r`   | rotate windows clockwise
`<SPC> w R`   | rotate windows counter-clockwise
`<SPC> w u`   | undo window layout (used to effectively undo a close window)
`<SPC> w U`   | redo window layout
`<SPC> w v`   | split a window vertically
`<SPC> w w`   | cycle and focus between windows

### Text manipulation commands

Text related commands (start with `x`):

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> x +`       | increase text font size
`<SPC> x -`       | decrease text font size
`<SPC> x =`       | reset text font size
`<SPC> x u`       | set the selected text to lower case
`<SPC> x U`       | set the selected text to upper case
`<SPC> x d w`     | delete trailing whitespaces
`<SPC> x g l`     | set languages used by translate commands
`<SPC> x g t`     | translate current word using Google Translate
`<SPC> x g T`     | reverse source and target languages
`<SPC> x m j`     | move down a line of text
`<SPC> x m k`     | move up a line of text
`<SPC> x t c`     | swap (transpose) the current character with the previous one
`<SPC> x t w`     | swap (transpose) the current word with the previous one
`<SPC> x t l`     | swap (transpose) the current line with the previous one
`<SPC> x w c`     | count the number of words in the selection region
`<SPC> x w C`     | count the number of occurrences per word in the select region

### Spell checking

Spell checking commands start with `s`:

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> s c`       | list of corrections in a `helm` buffer
`<SPC> s d`       | change dictionary language
`<SPC> s n`       | go to the next spell check error


### Region selection

Vi `Visual` modes are all supported by `evil`, `Spacemacs` adds another
`Visual` mode via the [expand-region][expand-region] mode.

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> v`     | initiate expand-region mode then...
`v`           | expand the region by one semantic unit
`V`           | contract the region by one semantic unit
`r`           | reset the region to initial selection
`ESC`         | leave expand-region mode

### Region narrowing

The displayed text of a buffer can be narrowed with the commands
(start with `n`):

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> n f`   | narrow the buffer to the current function
`<SPC> n p`   | narrow the buffer to the visible page
`<SPC> n r`   | narrow the buffer to the selected text
`<SPC> n w`   | widen, i.e show the whole buffer again

### Auto highlight

`Spacemacs` supports auto highlighting of the current word (provided by the
 [auto-highlight-symbol][auto-highlight] mode).

Key Binding   |                 Description
--------------|----------------------------------------------------------------
`<SPC> h e`   | edit all occurrences of the current word
`<SPC> h n`   | go to next occurrence
`<SPC> h p`   | go to previous occurrence
`<SPC> t h`   | toggle the auto highlighting

### Color theme

By default, `Spacemacs` uses the theme [Solarized][solarized-theme].

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> c t`       | cycle between available themes
`<SPC> h t`       | select a theme using a `helm` buffer

Available themes (other than default Emacs themes):
- [Solarized][solarized-theme]
- [Monokai][monokai-theme]
- [Zenburn][zenburn-theme]

### UI elements

#### Mode-line

The mode line is a [powerline][powerline] customized to show the window
number and the colorized Evil current state.

The color codes for modes are:

   Evil State     |       Color
------------------|------------------
Normal            | Orange
Insert            | Green
Visual            | Grey
Emacs             | Red
Motion            | Purple

When [Flycheck][flycheck] minor mode is enabled, the mode line color changes
according to the current flycheck state:

![mode line with solarized dark error](https://raw.github.com/syl20bnr/flycheck-color-mode-line/master/doc/flycheck-color-mode-line-dark-e.png)
![mode line with solarized dark warning](https://raw.github.com/syl20bnr/flycheck-color-mode-line/master/doc/flycheck-color-mode-line-dark-w.png)
![mode line with solarized dark warning](https://raw.github.com/syl20bnr/flycheck-color-mode-line/master/doc/flycheck-color-mode-line-dark-i.png)

#### Toggles

Some UI indicators can be toggled on and off (toggles start with `t`):

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> t 8`       | display a mark on the 80th column
`<SPC> t f`       | toggle display of the fringe
`<SPC> t n`       | show the line numbers (relative to current position)
`<SPC> t N`       | show the absolute line numbers

### Minor Modes

`Spacemacs` uses [diminish][diminish] mode to reduce the size of minor mode
indicators:

The minor mode area can be toggled on and off with (default is off):

    <SPC> t m

   Lighter   |                              Mode
-------------|-----------------------------------------------------------------
Ⓐ            | [auto-complete][auto-complete] mode
Ⓗ            | [auto-highlight-symbol][auto-highlight] mode
Ⓒ            | [centered-cursor][centered-cursor] mode
eⓅ           | [e-project][e-project] mode
Ⓟ            | [projectile][projectile] mode
Ⓕ            | flymake mode
Ⓢ            | flyspell mode
(Ⓢ)          | [smartparens][sp] mode
(Ⓟ)          | paredit mode
Ⓨ            | [yasnippet][yasnippet] mode

**Note:** in terminal the regular indicators are used instead of the utf-8
ones.

### Line formatting

Line formatting commands start with `j`:

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> j j`       | auto-indent the line below the current line and jump to it
`<SPC> j j`       | split the current line at point and auto-indent
`<SPC> j k`       | join the current line with the next line
`<SPC> l CTRL+j`  | split a quoted string

`<SPC> j k`, `<SPC> j j` and `<SPC> j i` used together are very powerful to quickly
reformat the code.

### Errors handling

`Spacemacs` uses [Flycheck][flycheck] to gives error feedback on the fly.
The checks are only performed at save time by default.

Errors management commands (star with `f` for `flycheck`):

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> f c`       | clear all errors
`<SPC> f l`       | display the `flycheck` list of errors/warnings
`<SPC> f n`       | go to the next `flycheck` error
`<SPC> f p`       | go to the previous flycheck error

### Project management

Projects in `Spacemacs` are managed with [projectile][projectile].
So projects are defined implicitly, for instance the root of a project
is found when a `.git` repository or `.projectile` file is encountered
in the file tree.

Projects management commands (start with `p`):

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> p b`       | switch to a buffer of the project 
`<SPC> p C`       | invalidate the cache of `projectile`
`<SPC> p d`       | open a `dired` buffer at the root of the project
`<SPC> p f`       | open a file of the project using `helm`
`<SPC> p F`       | find a file if the project using `ido`
`<SPC> p k`       | kill all the buffers of the project
`<SPC> p g`       | grep search in the project
`<SPC> p r`       | replace a string in the files of the project

### Modes

`Spacemacs` tries to add more natural Vi key bindings to some modes or
simply add new leader key bindings.

Leader key bindings start with `m` because they are bindings related to
the current `major mode`.

#### Helm

`Spacemacs` add `hjkl` navigation to `helm` buffers:

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`CTRL+h`          | go to previous page
`CTRL+j`          | go to previous item
`CTRL+k`          | go to next item
`CTRL+l`          | go to next page

#### Erlang

`Spacemacs` uses [EDTS][edts] as an Erlang coding environment.

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m d`       | show man page documentation
`<SPC> m e`       | go to next issue
`<SPC> m g`       | go to definition
`<SPC> m G`       | find a module in the current project
`<SPC> m h`       | open the header file under point
`<SPC> m l`       | find a function in the current module
`<SPC> m m`       | go to the macro definition under point
`<SPC> m r`       | go to the record definition under point

#### Ledger

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m a`       | add a transaction
`<SPC> m d`       | delete current transaction

#### Lisp

**TODO**

#### Magit

`Spacemacs` add `hjkl` navigation support for the following magit modes:
- branch manager
- commit
- log
- process
- status

**Note:** in `status` mode only `j` and `k` are remapped (to go down and up).
Press `K` instead of `k` to discard changes to an item.

#### Org

In `org`, [evil-org-mode][evil-org-mode] is activated.

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`gh`              | outline-up-heading
`gj`              | org-forward-heading-same-level
`gk`              | org-backward-heading-same-level
`gl`              | outline-next-visible-heading
`t`               | org-todo
`T`               | org-insert-todo-heading nil
`H`               | org-beginning-of-line
`L`               | org-end-of-line
`;t`              | org-show-todo-tree
`o`               | always-insert-item
`O`               | org-insert-heading
`$`               | org-end-of-line
`^`               | org-beginning-of-line
`<`               | org-metaleft
`>`               | org-metaright
`;a`              | org-agenda`

#### Perforce

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> p 4 a`     | add a file in depot
`<SPC> p 4 d`     | delete a file in depot
`<SPC> p 4 D`     | p4-describe
`<SPC> p 4 e`     | checkout a file
`<SPC> p 4 r`     | rename a file
`<SPC> p 4 R`     | revert a file
`<SPC> p 4 S`     | submit CL

#### Python

##### Inferior REPL process

Start an iPython inferior REPL process with `<SPC> m i`.

Send code to inferior process commands:

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m b`       | send buffer and keep code buffer focused
`<SPC> m B`       | send buffer and switch to REPL in insert mode
`<SPC> m f`       | send function and keep code buffer focused
`<SPC> m F`       | send function and switch to REPL in insert mode
`<SPC> m r`       | send region and keep code buffer focused
`<SPC> m R`       | send region and switch to REPL in insert mode
`CTRL+j`          | next item in REPL history
`CTRL+k`          | previous item in REPL history

##### Testing in Python

`Spacemacs` uses [nose][nose] as a test runner. An improved version of
[nose.el][nose.el] is shipped with `Spacemacs`, this version adds:
- windows support
- test suite support

The root of the project is detected with a `.git` directory or a `setup.cfg` file.

Test commands (start with `m t` or `m T`):

    No Debug      |                 Description
------------------|------------------------------------------------------------
<SPC> m t a       | launch all tests of the project
<SPC> m t f       | launch the current test under point
<SPC> m t m       | launch all tests of the current module
<SPC> m t s       | launch all tests of the current suite

     Debug        |                 Description
------------------|------------------------------------------------------------
<SPC> m T a       | launch all tests of the project in debug mode
<SPC> m T f       | launch the current test under point in debug mode
<SPC> m T m       | launch all tests of the current module in debug mode
<SPC> m T s       | launch all tests of the current suite in debug mode

##### Other Python commands

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m d`       | open documentation in `firefox` using [pylookup][pylookup]
`<SPC> m g`       | go to definition using [emacs-jedi][jedi]
`<SPC> m p`       | add a breakpoint

#### R (ESS)

**Important**:
In order to speed up the boot time of `Spacemacs`, `ESS` must be loaded
manually via the key binding `<SPC> l e`.

##### Inferior REPL process

Start an `R` inferior REPL process with `<SPC> m i`.

Send code to inferior process commands:

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m b`       | send buffer and keep code buffer focused
`<SPC> m B`       | send buffer and switch to REPL in insert mode
`<SPC> m f`       | send function and keep code buffer focused
`<SPC> m F`       | send function and switch to REPL in insert mode
`<SPC> m l`       | send line and keep code buffer focused
`<SPC> m L`       | send line and switch to REPL in insert mode
`<SPC> m r`       | send region and keep code buffer focused
`<SPC> m R`       | send region and switch to REPL in insert mode
`<SPC> m s`       | send region or line and step (debug)
`<SPC> m S`       | send function or paragraph and step (debug)
`CTRL+j`          | next item in REPL history
`CTRL+k`          | previous item in REPL history

##### Other R commands

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`<SPC> m p`       | object introspection popup [ess-R-object-popup][ess-R-object-popup]
`<SPC> m v p`     | view data under point using [ess-R-data-view][ess-R-data-view] 
`<SPC> m v t`     | view table using [ess-R-data-view][ess-R-data-view] 

#### rcirc

    Key Binding   |                 Description
------------------|------------------------------------------------------------
`CTRL+j`          | next item in command history
`CTRL+k`          | previous item in command history

### And more...

See [my-keybindings.el][keybindings] to explore for more key bindings.

## TODO list

- Add support for [multiple-cursors][multiple-cursors] mode.

[evil]: https://gitorious.org/evil/pages/Home
[evil-leader]: https://github.com/cofi/evil-leader
[RSI]: http://en.wikipedia.org/wiki/Repetitive_strain_injury
[sacha_guide]: http://sachachua.com/blog/2013/05/how-to-learn-emacs-a-hand-drawn-one-pager-for-beginners/
[use-package]: https://github.com/jwiegley/use-package
[keychords]: http://www.emacswiki.org/emacs/KeyChord
[centered-cursor]: http://www.emacswiki.org/emacs/centered-cursor-mode.el
[ace-jump]: https://github.com/winterTTr/ace-jump-mode
[helm]: https://github.com/emacs-helm/helm
[popwin]: http://www.emacswiki.org/emacs/PopWin
[golden-ratio]: https://github.com/roman/golden-ratio.el
[solarized-theme]: https://github.com/bbatsov/solarized-emacs
[powerline]: https://github.com/milkypostman/powerline
[diminish]: http://www.emacswiki.org/emacs/DiminishedModes
[auto-complete]: https://github.com/auto-complete
[auto-highlight]: https://github.com/emacsmirror/auto-highlight-symbol
[e-project]: https://github.com/jrockway/eproject
[projectile]: https://github.com/bbatsov/projectile
[sp]: https://github.com/Fuco1/smartparens
[flycheck]: https://github.com/flycheck
[yasnippet]: https://github.com/capitaomorte/yasnippet
[expand-region]: https://github.com/magnars/expand-region.el
[multiple-cursors]: https://github.com/magnars/multiple-cursors.el
[keybindings]: https://github.com/syl20bnr/vimacs/blob/master/my-keybindings.el
[hswoop]: https://github.com/ShingoFukuyama/helm-swoop
[hcss]: https://github.com/ShingoFukuyama/helm-css-scss
[hyas]: https://github.com/emacs-helm/helm-c-yasnippet
[hthemes]: https://github.com/syohex/emacs-helm-themes
[projectile]: https://github.com/bbatsov/projectile
[hdescbinds]: https://github.com/emacs-helm/helm-descbinds
[hflyspell]: https://gist.github.com/cofi/3013327
[evil-plugin01]: https://github.com/tarao/evil-plugins
[evil-plugin02]: https://github.com/redguardtoo/evil-matchit
[evil-plugin03]: https://github.com/bling/evil-visualstar
[evil-plugin04]: https://github.com/timcharper/evil-surround
[evil-plugin05]: https://github.com/Dewdrops/evil-exchange
[vim-plugin01]: http://www.vim.org/scripts/script.php?script_id=1905
[vim-plugin02]: http://www.vim.org/scripts/script.php?script_id=39
[vim-plugin03]: http://www.vim.org/scripts/script.php?script_id=1697
[vim-plugin04]: https://github.com/tommcdo/vim-exchange
[evil-org-mode]: https://github.com/edwtjo/evil-org-mode
[nose]: https://github.com/nose-devs/nose/
[nose.el]: https://github.com/syl20bnr/nose.el
[pylookup]: https://github.com/tsgates/pylookup
[jedi]: https://github.com/tkf/emacs-jedi
[edts]: https://github.com/tjarvstrand/edts
[ess-R-object-popup]: https://github.com/myuhe/ess-R-object-popup.el
[ess-R-data-view]: https://github.com/myuhe/ess-R-data-view.el
[monokai-theme]: https://github.com/oneKelvinSmith/monokai-emacs
[zenburn-theme]: https://github.com/bbatsov/zenburn-emacs
