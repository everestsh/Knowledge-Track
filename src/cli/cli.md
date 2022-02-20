# 命令行

假装「黑客」一样行事直到不希望被发现。😜

## 配置

- [starship](https://github.com/starship/starship) - Shell Prompt

## 工具

- [`tree`]() - 文件目录展示工具
- [`bat`](https://github.com/sharkdp/bat) - A cat(1) clone with syntax highlighting and Git integration.
- [`tokei`](https://github.com/XAMPPRocky/tokei) - 代码量统计工具
- [`tmux`](https://github.com/tmux/tmux) - 终端复用
	- [x] [A Quick and Easy Guide to tmux](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/)
	- [Tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
- [`fish`](https://fishshell.com/) - 交互更友好的 shell
	- [awsm.fish](https://github.com/jorgebucaran/awsm.fish)
- [asciinema](https://asciinema.org/) - Record and share your terminal sessions, the right way.
- [jq](https://stedolan.github.io/jq/) - JSON 处理
- [smartmontools](https://www.smartmontools.org/) - 查看硬盘健康状况，针对 M1 芯片硬盘读写问题
	<details><summary>示例</summary>
	```
	❯ smartctl -a disk0
	smartctl 7.2 2020-12-30 r5155 [Darwin 21.2.0 arm64] (local build)
	Copyright (C) 2002-20, Bruce Allen, Christian Franke, www.smartmontools.org
	
	=== START OF INFORMATION SECTION ===
	Model Number:                       APPLE SSD AP0512Q
	Serial Number:                      0ba0108b24244411
	Firmware Version:                   386.60.2
	PCI Vendor/Subsystem ID:            0x106b
	IEEE OUI Identifier:                0x000000
	Controller ID:                      0
	NVMe Version:                       1.2
	Number of Namespaces:               3
	Local Time is:                      Sun Feb 20 18:30:11 2022 CST
	Firmware Updates (0x02):            1 Slot
	Optional Admin Commands (0x0004):   Frmw_DL
	Optional NVM Commands (0x0004):     DS_Mngmt
	Maximum Data Transfer Size:         256 Pages
	
	Supported Power States
	St Op     Max   Active     Idle   RL RT WL WT  Ent_Lat  Ex_Lat
	 0 +     0.00W       -        -    0  0  0  0        0       0
	
	=== START OF SMART DATA SECTION ===
	SMART overall-health self-assessment test result: PASSED
	
	SMART/Health Information (NVMe Log 0x02)
	Critical Warning:                   0x00
	Temperature:                        27 Celsius
	Available Spare:                    100%
	Available Spare Threshold:          99%
	Percentage Used:                    1%
	Data Units Read:                    86,638,313 [44.3 TB]
	Data Units Written:                 69,117,640 [35.3 TB]
	Host Read Commands:                 639,771,714
	Host Write Commands:                297,254,752
	Controller Busy Time:               0
	Power Cycles:                       154
	Power On Hours:                     322
	Unsafe Shutdowns:                   31
	Media and Data Integrity Errors:    0
	Error Information Log Entries:      0
	
	Read 1 entries from Error Information Log failed: GetLogPage failed: system=0x38, sub=0x0, code=745
	```
	</details>

## Alias

```
alias workspace="open -a Xcode *.xcworkspace"
alias project="open -a Xcode *.xcodeproj"
```

## 链接

- [modern-unix](https://github.com/ibraheemdev/modern-unix) - A collection of modern/faster/saner alternatives to common unix commands.
- [ ] [Read The Tao of tmux | Leanpub](https://leanpub.com/the-tao-of-tmux/read)
- [Docker 命令行快捷指南](https://devhints.io/docker)
- [命令行的艺术](https://github.com/jlevy/the-art-of-command-line/blob/master/README-zh.md)
- [POSIX Shell and Utilities Quick Reference](http://shellhaters.org/)
- [Unofficial Guide to Dotfiles](https://dotfiles.github.io/) - Dotfiles 指南
- [x] [The best way to store your dotfiles: A bare Git repository](https://www.atlassian.com/git/tutorials/dotfiles)

### Git 版本控制

- [git - ours vs theirs](https://nitaym.github.io/ourstheirs/) - 更快捷优雅地处理冲突
- [[git-quick-checklist|Git 常用命令速查清单]]
- [x] [Learn Git Branching](https://learngitbranching.js.org/?locale=zh_CN) - Git 分支处理交互式教程
- [Pro Git](https://git-scm.com/book/zh/v2)
- [Tower- Learn Version Control with Git](https://www.git-tower.com/learn/git/ebook/cn/command-line/introduction)
- [图解Git](http://marklodato.github.io/visual-git-guide/index-zh-cn.html)
- [gitignore.io cli](https://docs.gitignore.io/install/command-line)

  <details><summary>速览</summary>

  安装：

  zhs
  ```sh
  echo "function gi() { curl -sLw "\n" https://www.toptal.com/  developers/gitignore/api/\$@ ;}" >> \
  ~/.zshrc && source ~/.zshrc
  ```

  使用：

  - `gi ios,swift >> .gitignore`
  - `gi list`
  </details>
- [Reduce repository size](https://docs.gitlab.com/ee/user/project/repository/reducing_the_repo_size_using_git.html)
