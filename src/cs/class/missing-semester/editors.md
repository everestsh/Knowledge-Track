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
