# The Missing Semester of Your CS Education

[计算机教育中缺失的一课](https://missing.csail.mit.edu/)

- [x] Course overview + the shell
- [x] Shell Tools and Scripting
- [ ] Editors (Vim)
- [ ] Data Wrangling
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

[WIP]

