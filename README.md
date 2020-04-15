# tweaks
Tweaks I've made to my stock Ubuntu OS install 

# video

Add support for tweaking the webcam settings

```bash
sudo apt install v4l-utils guvcview
```

# audio

Add the ability to support [IEC958 (S/PDIF)](https://askubuntu.com/q/1098666/3188) by installing the following packages:

```shell
sudo apt install libavresample-dev pavucontrol libasound2-plugins-extra
```

Control the settings via the Advanced tab of the relevant Output Device in the `pavucontrol` utility.

# shell

I use `bash`. Maybe some day I will switch to something nicer. I like the look of [`oil`](https://github.com/oilshell/oil) and [`nushell`](https://github.com/nushell/nushell/).

```console
$ cat ~/.bash_prompt 
#!/bin/bash

export PS1="[\D{%Y-%M-%d %H:%m:%S}] `whoami`@\h:`pwd``tput sgr0`\n$ \n\n"
```

```console
$ cat ~/.bashrc
# ~/.bashrc: executed by bash(1) for non-login shells.
# see /usr/share/doc/bash/examples/startup-files (in the package bash-doc)
# for examples

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# don't put duplicate lines or lines starting with space in the history.
# See bash(1) for more options
HISTCONTROL=ignoreboth

# append to the history file, don't overwrite it
shopt -s histappend

# for setting history length see HISTSIZE and HISTFILESIZE in bash(1)
HISTSIZE=1000
HISTFILESIZE=2000

# check the window size after each command and, if necessary,
# update the values of LINES and COLUMNS.
shopt -s checkwinsize

# If set, the pattern "**" used in a pathname expansion context will
# match all files and zero or more directories and subdirectories.
shopt -s globstar

# make less more friendly for non-text input files, see lesspipe(1)
[ -x /usr/bin/lesspipe ] && eval "$(SHELL=/bin/sh lesspipe)"

# set variable identifying the chroot you work in (used in the prompt below)
if [ -z "${debian_chroot:-}" ] && [ -r /etc/debian_chroot ]; then
    debian_chroot=$(cat /etc/debian_chroot)
fi

# set a fancy prompt (non-color, unless we know we "want" color)
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

# uncomment for a colored prompt, if the terminal has the capability; turned
# off by default to not distract the user: the focus in a terminal window
# should be on the output of commands, not on the prompt
#force_color_prompt=yes

if [ -n "$force_color_prompt" ]; then
    if [ -x /usr/bin/tput ] && tput setaf 1 >&/dev/null; then
	# We have color support; assume it's compliant with Ecma-48
	# (ISO/IEC-6429). (Lack of such support is extremely rare, and such
	# a case would tend to support setf rather than setaf.)
	color_prompt=yes
    else
	color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
unset color_prompt force_color_prompt

# If this is an xterm set the title to user@host:dir
case "$TERM" in
xterm*|rxvt*)
    PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
    ;;
*)
    ;;
esac

# enable color support of ls and also add handy aliases
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    #alias dir='dir --color=auto'
    #alias vdir='vdir --color=auto'

    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# colored GCC warnings and errors
#export GCC_COLORS='error=01;31:warning=01;35:note=01;36:caret=01;32:locus=01:quote=01'

# some more ls aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Add an "alert" alias for long running commands.  Use like so:
#   sleep 10; alert
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'

# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi


# Go!
export GOPATH=${HOME}/Work
export PATH="$GOPATH/bin:/home/tsm/Utilities:/home/tsm/Code/bin2src/target/release:$PATH"

#export PATH="/home/tsm/Tools/rr/bin:$PATH"

# GPG
export GPGKEY=BD4F8CAC
export GPG_TTY=$(tty)

# Virtualenv

export WORKON_HOME=~/.venv
source /usr/share/virtualenvwrapper/virtualenvwrapper.sh

# misc
export XDG_CONFIG_HOME=${HOME}
export JUJU_HOME=${GOPATH}/src/github.com/juju/juju

export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion

# Juju plugins and other bits and pieces
export PATH="$PATH:/home/tsm/Utilities"

# better bash history
# hat tip: https://sanctum.geek.nz/arabesque/better-bash-history/
shopt -s histappend
shopt -s cmdhist
export HISTFILESIZE=10000000 # 10 mb
export HISTSIZE=1000000 # 1 million
export HISTCONTROL=ignoreboth
export HISTIGNORE='ls:bg:fg:history'
export HISTTIMEFORMAT='%F %T '

# bash
export PROMPT_DIRTRIM=4
export PS1="[\D{%Y-%m-%d %H:%M:%S}] `whoami`@\h:\w`tput sgr0`\n$ "

# asciinema
#export PS1="$ "
#export PATH="/snap/bin:$PATH"

# Wasmer
export WASMER_DIR="/home/tsm/.wasmer"
[ -s "$WASMER_DIR/wasmer.sh" ] && source "$WASMER_DIR/wasmer.sh"
[2020-03-30 12:48:18] tsm@hawk:~
```

## extra packages

```console
sudo apt install \
  libxml2-utils
```

## snaps

```console
$ sudo snap install docker
$ sudo usermod -aG docker $USER
```

```console
$ sudo snap install microk8s
$ sudo usermod -aG microk8s $USER
```
