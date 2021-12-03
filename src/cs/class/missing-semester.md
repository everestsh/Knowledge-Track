# The Missing Semester of Your CS Education

[计算机教育中缺失的一课](https://missing.csail.mit.edu/)

- [x] Course overview + the shell
- [x] Shell Tools and Scripting
- [x] Editors (Vim)
- [x] Data Wrangling
- [ ] Command-line Environment
- [ ] Version Control (Git)
- [ ] Debugging and Profiling
- [ ] Metaprogramming
- [ ] Security and Cryptography
- [ ] Potpourri

## The Shell

- Basic usage
    - prompt
    - `~` (short for “home”)
    - `$` tells you that you are not the root user
    - execute a program (with arguments)
        - `date` · `echo` · `which`
    - argument: escape just the relevant characters with `\`
    - **environment variable**
        - `$PATH`
        - `:`-separated list of directories in `$PATH`
- Navigating in the shell
    - `/` · `cd` · `.` · `..` · `ls`
    - `mkdir` · `cp` · `mv` · `man`
    - `cat` · `tail`
- Connecting programs
    - redirection is `< file` and `> file`
    - use `>>` to append to a file
    - `|` operator lets you “chain” programs
- A versatile and powerful tool
    - `sudo`, lets you “do” something “as su” (short for “super user”, or “root”)
    - `find` 
    - `tee` - duplicate standard input
- More info & Exercises
    - `echo $SHELL`
    - `touch` & `man`
    - `#!/bin/sh` - ["shebang line(Unix)"](https://en.wikipedia.org/wiki/Shebang_(Unix))
    - the `sh` interpreter
    - `chmod`
    - [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/)
    - `|` & `>`

## Shell Tools and Scripting

- Shell Scripting
    - `foo=bar` differ from `foo = bar`

    - ```shell
      foo=bar
      echo "$foo"
      # prints bar
      echo '$foo'
      # prints $foo
      ```

    - `if`, `case`, `while` and `for` & function

    - ```shell
      mcd () {
          mkdir -p "$1"
          cd "$1"
      }
      ```

      - `$0` - Name of the script
      - `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
      - `$@` - All the arguments
      - `$#` - Number of arguments
      - `$?` - Return code of the previous command
      - `$$` - Process identification number (PID) for the current script
      - `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
      - `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.`
      - More info: [Chapter 3. Special Characters](https://tldp.org/LDP/abs/html/special-chars.html)

    - `&&` (and operator) and `||` (or operator)

    - `$( CMD )`, e.g: `for file in $(ls)`

    - `<( CMD )`, e.g: `diff <(ls foo) <(ls bar)`

    - [What is the difference between test, `[` and `[[` ?](https://mywiki.wooledge.org/BashFAQ/031)

    - shell *globbing*:

      - Wildcards: `?` & `*`
      - Curly braces: `{}`, expand, e.g: `convert image.{png,jpg}` => `convert image.png image.jpg`

    - [shellcheck](https://github.com/koalaman/shellcheck) - A shell script static analysis tool

    - `#!/PATH/TO/PROGRAME/` - ["shebang line(Unix)"](https://en.wikipedia.org/wiki/Shebang_(Unix))

      -  the [`env`](https://www.man7.org/linux/man-pages/man1/env.1.html) command

    - differences between shell functions and scripts

    - using [`export`](https://www.man7.org/linux/man-pages/man1/export.1p.html)

- Shell Tools

    - Finding how to use commands
        -  [`man`](https://www.man7.org/linux/man-pages/man1/man.1.html) 
        - For interactive tools such as the ones based on ncurses, help for the commands can often be accessed within the program using the `:help` command or typing `?`.
        -  [TLDR pages](https://tldr.sh/) 

    - Finding files

        - [`find`](https://www.man7.org/linux/man-pages/man1/find.1.html) or => [`fd`](https://github.com/sharkdp/fd) (simple, fast, and user-friendly alternative, in rust) 

        - ```shell
            # Find all directories named src
            find . -name src -type d
            # Find all python files that have a folder named test in their path
            find . -path '*/test/*.py' -type f
            # Find all files modified in the last day
            find . -mtime -1
            # Find all zip files with size in range 500k to 10M
            find . -size +500k -size -10M -name '*.tar.gz'
            ```

        - ```shell
          # Delete all files with .tmp extension
          find . -name '*.tmp' -exec rm {} \;
          # Find all PNG files and convert them to JPG
          find . -name '*.png' -exec convert {} {}.jpg \;
          
          ```

        - [`locate`](https://www.man7.org/linux/man-pages/man1/locate.1.html) , ref: [locate vs find: usage, pros and cons of each other](https://unix.stackexchange.com/questions/60205/locate-vs-find-usage-pros-and-cons-of-each-other)

    - Finding code

        - [`grep`](https://www.man7.org/linux/man-pages/man1/grep.1.html) - matching patterns from the input text

            -  `-C` for getting **C**ontext around the matching line
            - `-v` for in**v**erting the match
            - Alternative:  [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) and [rg](https://github.com/BurntSushi/ripgrep). 

        - ```shell
            # Find all python files where I used the requests library
            rg -t py 'import requests'
            # Find all files (including hidden files) without a shebang line
            rg -u --files-without-match "^#!"
            # Find all matches of foo and print the following 5 lines
            rg foo -A 5
            # Print statistics of matches (# of matched lines and files )
            rg --stats PATTERN
            ```

    - Finding shell commands

        - `history`
        - `Ctrl+R`, perform backwards search through your history
            - This can also be enabled with the UP/DOWN arrows in [zsh](https://github.com/zsh-users/zsh-history-substring-search). 
            - A nice addition on top of `Ctrl+R` comes with using [fzf](https://github.com/junegunn/fzf/wiki/Configuring-shell-key-bindings#ctrl-r) bindings. 
        - the [fish](https://fishshell.com/) shell
        -  `HISTCONTROL=ignorespace` to your `.bashrc` or `setopt HIST_IGNORE_SPACE` to your `.zshrc`. 
        - If you make the mistake of not adding the leading space, you can always manually remove the entry by editing your `.bash_history` or `.zhistory`.

    - Directory Navigation

        - writing shell aliases or creating symlinks with [ln -s](https://www.man7.org/linux/man-pages/man1/ln.1.html)
        - [`fasd`](https://github.com/clvv/fasd) and [`autojump`](https://github.com/wting/autojump).
        - get an overview of a directory structure: [`tree`](https://linux.die.net/man/1/tree), [`broot`](https://github.com/Canop/broot) 

    -  [`xargs`](https://www.man7.org/linux/man-pages/man1/xargs.1.html) command which will execute a command using STDIN as arguments

    - [using brew](https://formulae.brew.sh/formula/coreutils).

## Editors (Vim)

> Here’s how you learn a new editor:
>
> - Start with a tutorial (i.e. this lecture, plus resources that we point out)
> - Stick with using the editor for all your text editing needs (even if it slows you down initially)
> - Look things up as you go: if it seems like there should be a better way to do something, there probably is

- Philosophy of Vim

  - Vim is a *modal* editor: it has different **modes** for inserting text vs manipulating text. 
  - Vim is **programmable** (with Vimscript and also other languages like Python)
  - Vim’s interface itself is a programming language: **keystrokes** (with mnemonic names) are commands, and these commands are composable.
  - Vim avoids the use of the mouse, because it’s too slow; Vim even avoids using the arrow keys because it requires too much movement.

- Modal editing

  - Vim has multiple operating modes.
    - **Normal**: for moving around a file and making edits
    - **Insert**: for inserting text
    - **Replace**: for replacing text
    - **Visual** (plain, line, or block): for selecting blocks of text
    - **Command-line**: for running a command
  - change modes by pressing `<ESC>` (the escape key) to switch from any mode back to Normal mode. 
  - From Normal mode, 
    - enter Insert mode with `i`, 
    - Replace mode with `R`, 
    - Visual mode with `v`, 
    - Visual Line mode with `V`, 
    - Visual Block mode with `<C-v>` (Ctrl-V, sometimes also written `^V`), 
    - and Command-line mode with `:`.

- Basics

  - Inserting text. (though not particularly efficiently, if you’re spending all your time editing from Insert mode).
  - Buffers, tabs, and windows
  - Command-line
    - `:q` quit (close window)
    - `:w` save (“write”)
    - `:wq` save and quit
    - `:e {name of file}` open file for editing
    - `:ls` show open buffers
    - `:help {topic}` open help
      - `:help :w` opens help for the `:w` command
      - `:help w` opens help for the `w` movement

- Vim’s interface is a programming language

  - Movement("nouns")

    - Basic movement: `hjkl` (left, down, up, right)
    - Words: `w` (next word), `b` (beginning of word), `e` (end of word)
    - Lines: `0` (beginning of line), `^` (first non-blank character), `$` (end of line)
    - Screen: `H` (top of screen), `M` (middle of screen), `L` (bottom of screen)
    - Scroll: `Ctrl-u` (up), `Ctrl-d` (down)
    - File: `gg` (beginning of file), `G` (end of file)
    - Line numbers: `:{number}<CR>` or `{number}G` (line {number})
    - Misc: `%` (corresponding item)
    - Find: `f{character}`, `t{character}`, `F{character}`, `T{character}`
      - find/to forward/backward {character} on the current line
      - `,` / `;` for navigating matches
    - Search: `/{regex}`, `n` / `N` for navigating matches

  - Selection

    - Visual modes:
      - Visual: `v`
      - Visual Line: `V`
      - Visual Block: `Ctrl-v`
    - Can use movement keys to make selection.

  - Edits("verbs")

    - `i` enter Insert mode
      - but for manipulating/deleting text, want to use something more than backspace

    - `o` / `O` insert line below / above
    - `d{motion}` delete {motion}
      - e.g. `dw` is delete word, `d$` is delete to end of line, `d0` is delete to beginning of line
    - `c{motion}` change {motion}
      - e.g. `cw` is change word
      - like `d{motion}` followed by `i`
    - `x` delete character (equal do `dl`)
    - `s` substitute character (equal to `xi`)
    - Visual mode + manipulation
      - select text, `d` to delete it or `c` to change it
    - `u` to undo, `<C-r>` to redo
    - `y` to copy / “yank” (some other commands like `d` also copy)
    - `p` to paste
    - Lots more to learn: e.g. `~` flips the case of a character

  - Counts

    - combine nouns and verbs with a count
      - `3w` move 3 words forward
      - `5j` move 5 lines down
      - `7dw` delete 7 words

  - Modifiers

    - use modifiers to change the meaning of a noun. Some modifiers are `i`, which means “inner” or “inside”, and `a`, which means “around”.
      - `ci(` change the contents inside the current pair of parentheses
      - `ci[` change the contents inside the current pair of square brackets
      - `da'` delete a single-quoted string, including the surrounding single quotes

- Customizing Vim

  - Vim is customized through a plain-text configuration file in `~/.vimrc`
  - **Download our config [here](https://missing.csail.mit.edu/2020/files/vimrc) and save it to `~/.vimrc`.**
  - instructors’ Vim configs ([Anish](https://github.com/anishathalye/dotfiles/blob/master/vimrc), [Jon](https://github.com/jonhoo/configs/blob/master/editor/.config/nvim/init.vim) (uses [neovim](https://neovim.io/)), [Jose](https://github.com/JJGO/dotfiles/blob/master/vim/.vimrc))
    - Try not to copy-and-paste people’s full configuration, but read it, understand it, and **take what you need**.

- Extending Vim

  - Simply create the directory `~/.vim/pack/vendor/start/`, and put plugins in there
  - Here are some of our favorite plugins:
    - [ctrlp.vim](https://github.com/ctrlpvim/ctrlp.vim): fuzzy file finder
    - [ack.vim](https://github.com/mileszs/ack.vim): code search
    - [nerdtree](https://github.com/scrooloose/nerdtree): file explorer
    - [vim-easymotion](https://github.com/easymotion/vim-easymotion): magic motions
    - Check out [Vim Awesome](https://vimawesome.com/) for more awesome Vim plugins

- Vim-mode in other programs

  - Shell
    - If you’re a Bash user, use `set -o vi`. 
    - If you use Zsh, `bindkey -v`. 
    - For Fish, `fish_vi_key_bindings`. 
    - you can `export EDITOR=vim`. 
  - Readline
    - the [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library supports (basic) Vim emulation too, which can be enabled by adding the following line to the `~/.inputrc` file:`set editing-mode vi`
  - Others
    -  [Vimium](https://chrome.google.com/webstore/detail/vimium/dbepggeogbaibhgnhhndojpepiihcmeb?hl=en) for Google Chrome
    - a [long list](https://reversed.top/2016-08-13/big-list-of-vim-like-software) of software with vim-like keybindings.

- Advanced Vim

  - Search and replace

    - `:s` (substitute) command ([documentation](http://vim.wikia.com/wiki/Search_and_replace)).

      - ```plaintext
        %s/foo/bar/g
        ```

        - replace foo with bar globally in file

      - ```plaintext
        %s/\[.*\](\(.*\))/\1/g
        ```

        - replace named Markdown links with plain URLs

  - Multiple windows

    - `:sp` / `:vsp` to split windows
    - Can have multiple views of the same buffer.

  - Macros (More Advanced)

    - // To be continued

- Resources

  - `vimtutor` is a tutorial that comes installed with Vim - if Vim is installed, you should be able to run `vimtutor` from your shell
  - [Vim Adventures](https://vim-adventures.com/) is a game to learn Vim
  - [Vim Tips Wiki](http://vim.wikia.com/wiki/Vim_Tips_Wiki)
  - [Vim Advent Calendar](https://vimways.org/2019/) has various Vim tips
  - [Vim Golf](http://www.vimgolf.com/) is [code golf](https://en.wikipedia.org/wiki/Code_golf), but where the programming language is Vim’s UI
  - [Vi/Vim Stack Exchange](https://vi.stackexchange.com/)
  - [Vim Screencasts](http://vimcasts.org/)
  - [Practical Vim](https://pragprog.com/titles/dnvim2/) (book)

## Data Wrangling

- Overview

  - `less` gives us a “pager” that allows us to scroll up and down through the long output. 
  - `sed` is a “stream editor” that builds on top of the old `ed` editor. 

- Regular expressions

  - surrounded by `/`. 
  - Very common patterns are:
    - `.` means “any single character” except newline
    - `*` zero or more of the preceding match
    - `+` one or more of the preceding match
    - `[abc]` any one character of `a`, `b`, and `c`
    - `(RX1|RX2)` either something that matches `RX1` or `RX2`
    - `^` the start of the line
    - `$` the end of the line
  - https://regex101.com/
  - https://www.regular-expressions.info/

- Back to data wrangling

  - `uniq -c` will collapse consecutive lines that are the same into a single line
  - `sort` will, well, sort its input
  -  `head` / `tail`.

- awk – another editor

  - `awk` is a programming language that just happens to be really good at processing text streams.

  - ```shell
     | awk '$1 == 1 && $2 ~ /^c[^ ]*e$/ { print $2 }' | wc -l
    ```

  - ```shell
    BEGIN { rows = 0 }
    $1 == 1 && $2 ~ /^c[^ ]*e$/ { rows += $1 }
    END { print rows }
    ```

  - [Idiomatic awk](https://backreference.org/2010/02/10/idiomatic-awk/)

- Analyzing data

  - `bc` - a calculator 
  - R is another (weird) programming language that’s great at data analysis and [plotting](https://ggplot2.tidyverse.org/). 

- Data wrangling to make arguments

  - `xargs` 

  - ```shell
    rustup toolchain list | grep nightly | grep -vE "nightly-x86" | sed 's/-x86.*//' | xargs rustup toolchain uninstall
    ```

- Wrangling binary data

  - use ffmpeg to capture an image from our camera, convert it to grayscale, compress it, send it to a remote machine over SSH, decompress it there, make a copy, and then display it.

  - ```shell
    ffmpeg -loglevel panic -i /dev/video0 -frames 1 -f image2 -
     | convert - -colorspace gray -
     | gzip
     | ssh mymachine 'gzip -d | tee copy.jpg | env DISPLAY=:0 feh -'
    ```

- [ ] Take this [short interactive regex tutorial](https://regexone.com/).
- [ ] For JSON data, try [`jq`](https://stedolan.github.io/jq/).

## Command-line Environment

[WIP]

