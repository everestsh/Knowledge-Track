# [Homebrew](https://brew.sh/)

> macOS 包管理大杀器
> The Missing Package Manager for macOS (or Linux)

## 安装

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

### [自动补全](https://docs.brew.sh/Shell-Completion#configuring-completions-in-zsh)

- Ohmyzsh

    添加以下一行添加到`~/.zprofile`

```
FPATH=$(brew --prefix)/share/zsh/site-functions:$FPATH
```

## 常用命令

- Example usage:
  - brew search [TEXT|/REGEX/]
  - brew info [FORMULA...]
  - brew install FORMULA...
  - brew update
  - brew upgrade [FORMULA...]
  - brew uninstall FORMULA...
  - brew list [FORMULA...]

- Troubleshooting:
  - brew config
  - brew doctor
  - brew install --verbose --debug FORMULA

- Contributing:
  - brew create [URL [--no-fetch]]
  - brew edit [FORMULA...]

- Further help:
  - brew commands
  - brew help [COMMAND]
  - man brew
  - https://docs.brew.sh

## 其他
避免触发自动更新
```sh
export HOMEBREW_NO_AUTO_UPDATE=1
```