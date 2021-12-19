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

  > Since [premature optimization is the root of all evil](http://wiki.c2.com/?PrematureOptimization), you should learn about profilers and monitoring tools. 
  
  - Timing
    - *Real* time, wall clock time can be misleading
    - In general, *User* + *Sys* tells you how much time your process actually spent in the CPU
      - *Real* - Wall clock elapsed time from start to finish of the program, including the time taken by other processes and time taken while blocked (e.g. waiting for I/O or network)
      - *User* - Amount of time spent in the CPU running user code
      - *Sys* - Amount of time spent in the CPU running kernel code
    - e.g: `time curl https://missing.csail.mit.edu &> /dev/null`
  - Profilers
    - CPU
      - *tracing* and *sampling* profilers. [[more](https://jvns.ca/blog/2017/12/17/how-do-ruby---python-profilers-work-/)]
      - `cProfile`,  [`line_profiler`](https://github.com/pyutils/line_profiler) in Python
    - Memory
      - memory leaks: like [Valgrind](https://valgrind.org/) that will help you identify memory leaks.
    - Event Profiling
      - The [`perf`](https://www.man7.org/linux/man-pages/man1/perf.1.html) command can easily report poor cache locality, high amounts of page faults or livelocks
      - `perf list` - List the events that can be traced with perf
      - `perf stat COMMAND ARG1 ARG2` - Gets counts of different events related a process or command
      - `perf record COMMAND ARG1 ARG2` - Records the run of a command and saves the statistical data into a file called `perf.data`
      - `perf report` - Formats and prints the data collected in `perf.data`
    - Visualization
      - displaying profiler’s output in an easier to parse way.
      - display CPU profiling information for sampling profilers is to use a [Flame Graph](http://www.brendangregg.com/flamegraphs.html)
      - **Call graphs** or **control flow graphs** display the relationships between subroutines within a program by including functions as nodes and functions calls between them as directed edges.
  - Resource Monitoring
    - **General Monitoring**:  [`htop`](https://htop.dev/) / [`top`](https://www.man7.org/linux/man-pages/man1/top.1.html) / [`glances`](https://nicolargo.github.io/glances/)
    - **I/O operations**: [`iotop`](https://www.man7.org/linux/man-pages/man8/iotop.8.html)
    - **Disk Usage**:  [`df`](https://www.man7.org/linux/man-pages/man1/df.1.html) displays metrics per partitions and [`du`](http://man7.org/linux/man-pages/man1/du.1.html) displays **d**isk **u**sage per file for the current directory.  `-h` flag
      - A more interactive version of `du` is [`ncdu`](https://dev.yorhel.nl/ncdu) 
    - **Memory Usage**:  [`free`](https://www.man7.org/linux/man-pages/man1/free.1.html) displays the total amount of free and used memory in the system
    - **Open Files**: [`lsof`](https://www.man7.org/linux/man-pages/man8/lsof.8.html) lists file information about files opened by processes.
    - **Network Connections and Config**:
      -  [`ss`](https://www.man7.org/linux/man-pages/man8/ss.8.html) lets you monitor incoming and outgoing network packets statistics.
      -  For displaying routing, network devices and interfaces you can use [`ip`](http://man7.org/linux/man-pages/man8/ip.8.html).
      - `netstat` and `ifconfig` have been deprecated
    - **Network Usage**: [`nethogs`](https://github.com/raboof/nethogs) and [`iftop`](http://www.ex-parrot.com/pdw/iftop/)
    - The [`stress`](https://linux.die.net/man/1/stress) command.
  - Specialized tools
    - Tools like [`hyperfine`](https://github.com/sharkdp/hyperfine) let you quickly benchmark command line programs. 
      - `hyperfine --warmup 3 'fd -e jpg' 'find . -iname "*.jpg"'`
    - Webpage loading: More info for [Firefox](https://profiler.firefox.com/docs/) and [Chrome](https://developers.google.com/web/tools/chrome-devtools/rendering-tools).

