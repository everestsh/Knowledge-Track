## Debugging and Profiling

- Debugging

  - Printf debugging and Logging

    - Logging is better than regular print statements for several reasons：
      - You can log to files, sockets or even remote servers instead of standard output.
      - Logging supports **severity levels** (such as INFO, DEBUG, WARN, ERROR, &c), that allow you to filter the output accordingly.
      - For new issues, there’s a fair chance that your logs will contain enough information to detect what is going wrong.
    - color code them
      - executing `echo -e "\e[38;2;255;0;0mThis is red\e[0m"` prints the message `This is red` in red on your terminal, as long as it supports true color. 

  - Third party logs

    - In UNIX systems, logs commonplace: `/var/log`

    - In most Linux: `systemd` & `journalctl` 

    - on macOS there is still `/var/log/system.log`, `log show`

    - On most UNIX systems, use the `dmesg` to access the kernel log.

    - use `logger` to logging under system log:

    - ```sh
      logger "Hello Logs"
      # On macOS
      log show --last 1m | grep Hello
      # On Linux
      journalctl --since "1m ago" | grep Hello
      ```

    - tools like [`lnav`](http://lnav.org/), that provide an improved presentation and navigation for log files.

  - Debuggers

    - Debuggers are programs that let you interact with the execution of a program:
      - Halt execution of the program when it reaches a certain line.
      - Step through the program one instruction at a time.
      - Inspect values of variables after the program crashed.
      - Conditionally halt the execution when a given condition is met.
      - And many more advanced features
    - Python Debugger [`pdb`](https://docs.python.org/3/library/pdb.html).
    -  [`ipdb`](https://pypi.org/project/ipdb/) is an improved `pdb` that uses the [`IPython`](https://ipython.org/) REPL
      - tab completion, 
      - syntax highlighting, 
      - better tracebacks, and better introspection 
      - retaining the same interface as the `pdb` module.
    - more low level programming you will probably want to look into [`gdb`](https://www.gnu.org/software/gdb/) (and its quality of life modification [`pwndbg`](https://github.com/pwndbg/pwndbg)) and [`lldb`](https://lldb.llvm.org/). 

  - Specialized Tools

    - black box binary

    - trace the syscalls your program makes

    - In Linux there’s [`strace`](https://www.man7.org/linux/man-pages/man1/strace.1.html) and macOS and BSD have [`dtrace`](http://dtrace.org/blogs/about/).

    - ```sh
      # On Linux
      sudo strace -e lstat ls -l > /dev/null
      4
      # On macOS
      sudo dtruss -t lstat64_extended ls -l > /dev/null
      ```

    - look at the network packets

      - Tools like [`tcpdump`](https://www.man7.org/linux/man-pages/man1/tcpdump.1.html) and [Wireshark](https://www.wireshark.org/) are network packet analyzers

    - For web development, the Chrome/Firefox developer tools

  - Static Analysis

    - Python:  [`pyflakes`](https://pypi.org/project/pyflakes) /  [`mypy`](http://mypy-lang.org/) 
    - Shell scripts:  [`shellcheck`](https://www.shellcheck.net/)
    - Code linting
    - Vim plugins:  [`ale`](https://vimawesome.com/plugin/ale) or [`syntastic`](https://vimawesome.com/plugin/syntastic) 
    - [Awesome Static Analysis](https://github.com/mre/awesome-static-analysis) & [Awesome Linters](https://github.com/caramelomartins/awesome-linters)
    - Code formatters

- Profiling

  - 🚧 
