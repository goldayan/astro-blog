---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: ZSH Config
description: My simple and effective zsh config
date: 2022-03-03
author: "Gold Ayan"
category: "Coding"
tags:
  - zsh
---

Apple changed the default shell to zsh from bash for quite sometime. let's
customize it.People often use [OH MY ZSH](https://ohmyz.sh/), I used it for a year but it seems bloated to
me. Below is my simple and effective configuration.

<!-- more -->

## Changing the shell prompt to **Starship**

- Default prompt is ugly
- Starship is the best i used.[period]
- Install starship from https://starship.rs/
- Starship toml file config (~/.config/starship.toml)

```toml
add_newline = false

[character]
success_symbol = "[➜](bold green)"

# Clear visibility for exit codes
[status]
style = "bg:red"
symbol = "💣 "
format = '[\[$symbol$status\]]($style) '
disabled = false

# These symbols will only show when battery levels are 20% or less
[battery]
charging_symbol = "⚡️ "
discharging_symbol = "💀 "

[[battery.display]]  # "bold red" style when capacity is between 0% and 20%
threshold = 40
style = "bold red"

#[python]
#python_binary = "python3"

[swift]
symbol = "ﯣ (orange)"
```

- I have added a warning when my battery is below 40 to warn me and swift language symbol.
- Add the following line in .zshrc to start starship when shell initialize.

```sh
eval "$(starship init zsh)"
```

## Key Bindings Changes

- Edit Command in nvim (choose the editor you like)

```sh
export EDITOR=nvim
```

- Bind edit command line command to Ctrl+x plus Ctrl+e [Work in Progress]

```sh
bindkey '^X^e' edit-command-line
```

- Once we set editor as vim, zsh changes bindings to vi, override with emacs bindings

```sh
bindkey -e
```

- Check all the key bindings

```sh
bindkey | less
```

reference: https://matija.suklje.name/zsh-vi-and-emacs-modes

## ZSH autosuggestion

- I like to see suggestion(taken from previous bash history) when typing, ZSH Autosuggestion does that.
- Now i can't work without it.

```sh
source ~/.zsh/zsh-autosuggestions/zsh-autosuggestions.zsh
```

## Z Script Integration

- Easily jump between directories easily.
- src: https://github.com/rupa/z/
- I have copied the `z.sh` from the above github and place it `/Users/sample/Scripts/Production/z.sh`
- Adding it to the .zshrc

```sh
. /Users/sample/Scripts/Production/z.sh
```

## History override

- Override history threshold.

```sh
HIST_STAMPS="dd.mm.yyyy"
HISTFILE=~/.histfile
HISTSIZE=100000
SAVEHIST=100000
```

## FZF Integration

- FZF integration with shell.
- I favourite is `ctrl+r`, search through history with FZF.

```sh
[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
```

## NeoVim as Manpager [Optional]

- Using neovim as Man page viewer (Why not? :p)

```sh
export MANPAGER="nvim -c 'set ft=man' -"
```

## Alias

- Created alias to edit and source Zshell config
- Customize to your likings.

```sh
alias zshcnf="nvim ~/.zshrc"
alias zshsrc="source ~/.zshrc"
```
