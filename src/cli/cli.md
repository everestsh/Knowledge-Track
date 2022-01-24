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
