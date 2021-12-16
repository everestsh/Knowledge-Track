## Command-line Environment

- Job Control

  - Killing a process
    - `Ctrl-C`: deliver a SIGINT signal to the process.
    - use the SIGQUIT signal instead, by typing `Ctrl-\`
    - Note that `^` is how `Ctrl` is displayed when typed in the terminal.
    - a more generic signal for asking a process to exit gracefully is the `SIGTERM` signal
      - `kill -TERM <PID>`
    
  - Pausing and backgrounding processes
    - typing `Ctrl-Z` will prompt the shell to send a `SIGTSTP` signal
    
    - continue the paused job in the foreground or in the background using [`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html) or [`bg`](http://man7.org/linux/man-pages/man1/bg.1p.html)
    - The [`jobs`](https://www.man7.org/linux/man-pages/man1/jobs.1p.html) command lists the unfinished jobs associated with the current terminal session.
      - use [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) to find that out
    - refer to the last backgrounded job you can use the `$!` special parameter
    - the `&` suffix in a command will run the command in the background
    - To background an already running program you can do `Ctrl-Z` followed by `bg`.
      - `SIGHUP`
      -  [`nohup`](https://www.man7.org/linux/man-pages/man1/nohup.1.html) (a wrapper to ignore `SIGHUP`), or use `disown` if the process has already been started
    - other signals [here](https://en.wikipedia.org/wiki/Signal_(IPC)) or typing [`man signal`](https://www.man7.org/linux/man-pages/man7/signal.7.html) or `kill -l`.

- Terminal Multiplexers

  > Terminal multiplexers like [`tmux`](https://www.man7.org/linux/man-pages/man1/tmux.1.html) allow you to multiplex terminal windows using panes and tabs so you can interact with multiple shell sessions.

  - keybindings: the form `<C-b> x` where that means (1) press `Ctrl+b`, (2) release `Ctrl+b`, and then (3) press `x`. 
  - hierarchy of objects:
    - **Sessions** - an independent workspace with one or more windows
      - `tmux` starts a new session.
      - `tmux new -s NAME` starts it with that name.
      - `tmux ls` lists the current sessions
      - Within `tmux` typing `<C-b> d` **detaches** the current session
      - `tmux a` **attaches** the last session. You can use `-t` flag to specify which
    - **Windows** - tabs in editors or browsers, they are visually separate parts of the same session
      - `<C-b> c` **Creates** a new window. To **close** it you can just terminate the shells doing `<C-d>`
      - `<C-b> N` Go to the *N* th window. Note they are numbered
      - `<C-b> p` Goes to the previous window
      - `<C-b> n` Goes to the next window
      - `<C-b> ,` Rename the current window
      - `<C-b> w` List current windows
    - **Panes** - Like vim splits, panes let you have multiple shells in the same visual display.
      - `<C-b> "` Split the current pane horizontally
      - `<C-b> %` Split the current pane vertically
      - `<C-b> <direction>` Move to the pane in the specified *direction*. Direction here means arrow keys.
      - `<C-b> z` Toggle zoom for the current pane
      - `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection.
      - `<C-b> <space>` Cycle through pane arrangements.
  -  [here](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) is a quick tutorial on `tmux` and [this](http://linuxcommand.org/lc3_adv_termmux.php) has a more detailed explanation that covers the original `screen` command.

- Aliases

  - Aliases have many convenient features:

  - ```
    # Make shorthands for common flags
    alias ll="ls -lh"
    
    # Save a lot of typing for common commands
    alias gs="git status"
    alias gc="git commit"
    alias v="vim"
    
    # Save you from mistyping
    alias sl=ls
    
    # Overwrite existing commands for better defaults
    alias mv="mv -i"           # -i prompts before overwrite
    alias mkdir="mkdir -p"     # -p make parent dirs as needed
    alias df="df -h"           # -h prints human readable format
    
    # Alias can be composed
    alias la="ls -A"
    alias lla="la -l"
    
    # To ignore an alias run it prepended with \
    \ls
    # Or disable an alias altogether with unalias
    unalias la
    
    # To get an alias definition just call it with alias
    alias ll
    # Will print ll='ls -lh'
    ```

  - include it in shell startup files, like `.bashrc` or `.zshrc`, 

- Dotfiles

  - For `bash`, editing your `.bashrc` or `.bash_profile` will work in most systems.
    - `PATH` environment, like `export PATH="$PATH:/path/to/program/bin"`

  - `git` - `~/.gitconfig`
  - `vim` - `~/.vimrc` and the `~/.vim` folder
  - `ssh` - `~/.ssh/config`
  - `tmux` - `~/.tmux.conf`
  - in their own folder, under version control, and **symlinked** into place using a script
  - Portability

- Remote Machines

  - SSH is highly configurable so it is worth learning about it.

  - SSH Keys:  `~/.ssh/id_rsa`

    - Key generation:  run [`ssh-keygen`](https://www.man7.org/linux/man-pages/man1/ssh-keygen.1.html).

      - ```shell
        ssh-keygen -o -a 100 -t ed25519 -f ~/.ssh/id_ed25519
        ```

    - Key based authentication
      - `ssh` will look into `.ssh/authorized_keys` to determine which clients it should let in
      - `ssh-copy-id`

  - Copying files over SSH

    - `ssh+tee`
    - [`scp`](https://www.man7.org/linux/man-pages/man1/scp.1.html) when copying large amounts of files/directories
    - [`rsync`](https://www.man7.org/linux/man-pages/man1/rsync.1.html) improves upon `scp` by detecting identical files in local and remote, and preventing copying them again.

  - Port Forwarding

    - SSH Configuration

    - ```
      alias my_server="ssh -i ~/.id_ed25519 --port 2222 -L 9999:localhost:8888 foobar@remote_server
      ```

      - a better alternative using `~/.ssh/config`.
      - be thoughtful about sharing your SSH configuration.

- Shells & Frameworks

  - `bash`, `zsh`/ [fish](https://fishshell.com/)/
  - general frameworks are [prezto](https://github.com/sorin-ionescu/prezto) or [oh-my-zsh](https://ohmyz.sh/)
  -  [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting) or [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search)
  - One thing to note when using these frameworks is that they may slow down your shell

- Terminal Emulators

  - Font choice
  - Color Scheme
  - Keyboard shortcuts
  - Tab/Pane support
  - Scrollback configuration
  - Performance (some newer terminals like [Alacritty](https://github.com/jwilm/alacritty) or [kitty](https://sw.kovidgoyal.net/kitty/) offer GPU acceleration).

