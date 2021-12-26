# Potpourri

- Keyboard remapping, e.g:

  - Remap `Caps Lock` to `Ctrl` or `Escape`.
  - Remapping `PrtSc` to `Play/Pause` music.
  - Swapping `Ctrl` and the `Meta` (Windows or Command) key.
  - Map keys to arbitrary commands of your choosing, e.g:
    - Open a new terminal or browser window.
    - Inserting some specific text, e.g. your long email address or your MIT ID number.
    - Sleeping the computer or the displays.
  - Software resources:
    - macOS - [**karabiner-elements**](https://pqrs.org/osx/karabiner/), [skhd](https://github.com/koekeishiya/skhd) or [BetterTouchTool](https://folivora.ai/)
    - Linux - [xmodmap](https://wiki.archlinux.org/index.php/Xmodmap) or [Autokey](https://github.com/autokey/autokey)
    - Windows - Builtin in Control Panel, [AutoHotkey](https://www.autohotkey.com/) or [SharpKeys](https://www.randyrants.com/category/sharpkeys/)
    - QMK - If your keyboard supports custom firmware you can use [QMK](https://docs.qmk.fm/) to configure the hardware device itself so the remaps works for any machine you use the keyboard with.

- Daemons

  - These processes are called daemons and the programs that run as daemons often end with a `d` to indicate so.
  - Systemd can be interacted with the `systemctl` command in order to `enable`, `disable`, `start`, `stop`, `restart` or check the `status` of services
  - use [`cron`](https://www.man7.org/linux/man-pages/man8/cron.8.html), a daemon your system already runs to perform scheduled tasks.

- FUSE

  - [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace) (Filesystem in User Space) allows filesystems to be implemented by a user program.
  - Examples:
    - [sshfs](https://github.com/libfuse/sshfs) - Open locally remote files/folder through an SSH connection.
    - [rclone](https://rclone.org/commands/rclone_mount/) - Mount cloud storage services like Dropbox, GDrive, Amazon S3 or Google Cloud Storage and open data locally.
    - [gocryptfs](https://nuetzlich.net/gocryptfs/) - Encrypted overlay system. Files are stored encrypted but once the FS is mounted they appear as plaintext in the mountpoint.
    - [kbfs](https://keybase.io/docs/kbfs) - Distributed filesystem with end-to-end encryption. You can have private, shared and public folders.
    - [borgbackup](https://borgbackup.readthedocs.io/en/stable/usage/mount.html) - Mount your deduplicated, compressed and encrypted backups for ease of browsing.

- Backups

  - Synchronization solutions are not backups.

  - [ ] For a more detailed explanation, see 2019’s lecture notes on [Backups](https://missing.csail.mit.edu/2019/backups).

- APIs

  - With JSON, you can pipe through a tool like [`jq`](https://stedolan.github.io/jq/) to massage into what you care about.
  - Authentication,  “[OAuth](https://www.oauth.com/)”
  - [IFTTT](https://ifttt.com/) 

- Common command-line flags/patterns

  - Most tools support some kind of `--help` flag to display brief usage instructions for the tool.
  - Many tools that can cause irrevocable change support the notion of a “dry run” in which they only print what they *would have done*, but do not actually perform the change. Similarly, they often have an “interactive” flag that will prompt you for each destructive action.
  - You can usually use `--version` or `-V` to have the program print its own version (handy for reporting bugs!).
  - Almost all tools have a `--verbose` or `-v` flag to produce more verbose output. You can usually include the flag multiple times (`-vvv`) to get *more* verbose output, which can be handy for debugging. Similarly, many tools have a `--quiet` flag for making it only print something on error.
  - In many tools, `-` in place of a file name means “standard input” or “standard output”, depending on the argument.
  - Possibly destructive tools are generally not recursive by default, but support a “recursive” flag (often `-r`) to make them recurse.
  - Sometimes, you want to pass something that *looks* like a flag as a normal argument. For example, imagine you wanted to remove a file called `-r`. Or you want to run one program “through” another, like `ssh machine foo`, and you want to pass a flag to the “inner” program (`foo`). The special argument `--` makes a program *stop* processing flags and options (things starting with `-`) in what follows, letting you pass things that look like flags without them being interpreted as such: `rm -- -r` or `ssh machine --for-ssh -- foo --for-foo`.

- Window managers
  - My option: Magnet.
- VPNs
  - Considerations：[Don't use VPN services.](https://gist.github.com/joepie91/5a9909939e6ce7d09e29)
- Markdown
  - Instead of pulling out a heavy tool like Word or LaTeX, you may want to consider using the lightweight markup language [Markdown](https://commonmark.org/help/).
- Hammerspoon (desktop automation on macOS)
  - [Hammerspoon](https://www.hammerspoon.org/) is a desktop automation framework for macOS.
  - Examples：
    - Bind hotkeys to move windows to specific locations
    - Create a menu bar button that automatically lays out windows in a specific layout
    - Mute your speaker when you arrive in lab (by detecting the WiFi network)
    - Show you a warning if you’ve accidentally taken your friend’s power supply
- Booting + Live USBs
  - [Live USBs](https://en.wikipedia.org/wiki/Live_USB) are USB flash drives containing an operating system.
    - There are tools like [UNetbootin](https://unetbootin.github.io/) to help you create live USBs.
- Docker, Vagrant, VMs, Cloud, OpenStack
  - [Vagrant](https://www.vagrantup.com/) is a tool that lets you describe machine configurations
  - [Docker](https://www.docker.com/) is conceptually similar but it uses containers instead.
  - Virtual Machines on the cloud：
    - Popular services include [Amazon AWS](https://aws.amazon.com/), [Google Cloud](https://cloud.google.com/),[ Microsoft Azure](https://azure.microsoft.com/), [DigitalOcean](https://www.digitalocean.com/).
- Notebook programming
  - [Notebook programming environments](https://en.wikipedia.org/wiki/Notebook_interface) can be really handy for doing certain types of interactive or exploratory development.
  -  [Jupyter](https://jupyter.org/), for Python
  -  [Wolfram Mathematica](https://www.wolfram.com/mathematica/) is another notebook programming environment that’s great for doing math-oriented programming.

- GitHub
  - There are two primary ways in which people contribute to projects on GitHub:
    - Creating an [issue](https://help.github.com/en/github/managing-your-work-on-github/creating-an-issue). 
    - Contribute code through a [pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests).

