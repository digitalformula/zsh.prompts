#
# Agnoster theme from https://gist.github.com/3712874
#
# Authors:
#   agnoster (original)
#	digitalformula (modified for compatibility with Prezto - https://github.com/sorin-ionescu/prezto)
#
# Original agnoster theme was written for Oh My Zsh - https://github.com/robbyrussell/oh-my-zsh
# This modified version requires git.zsh from Oh My Zsh - put in ~/.yadr/zsh for automatic load

# Load dependencies.
pmodload 'helper'

function prompt_agnoster_precmd {
  setopt LOCAL_OPTIONS
  unsetopt XTRACE KSH_ARRAYS

  if (( $+functions[git-info] )); then
    git-info
  fi
}

# Begin a segment
# Takes two arguments, background and foreground. Both can be omitted,
# rendering default background/foreground.
prompt_segment() {
  local bg fg
  [[ -n $1 ]] && bg="%K{$1}" || bg="%k"
  [[ -n $2 ]] && fg="%F{$2}" || fg="%f"
  if [[ $CURRENT_BG != 'NONE' && $1 != $CURRENT_BG ]]; then
    echo -n " %{$bg%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR%{$fg%} "
  else
    echo -n "%{$bg%}%{$fg%} "
  fi
  CURRENT_BG=$1
  [[ -n $3 ]] && echo -n $3
}

# End the prompt, closing any open segments
prompt_end() {
  if [[ -n $CURRENT_BG ]]; then
    echo -n " %{%k%F{$CURRENT_BG}%}$SEGMENT_SEPARATOR"
  else
    echo -n "%{%k%}"
  fi
  echo -n "%{%f%}"
  CURRENT_BG=''
}

### Prompt components
# Each component will draw itself, and hide itself if no information needs to be shown

# Context: user@hostname (who am I and where am I)
prompt_context() {
  local user=`whoami`

  if [[ "$user" != "$DEFAULT_USER" || -n "$SSH_CLIENT" ]]; then
    prompt_segment black default "%(!.%{%F{yellow}%}.)$user@%m"
  fi
}

# Git: branch/detached head, dirty status
prompt_git() {
  local ref dirty
  if $(git rev-parse --is-inside-work-tree >/dev/null 2>&1); then
    ZSH_THEME_GIT_PROMPT_DIRTY='±'
    dirty=$(parse_git_dirty)
    ref=$(git symbolic-ref HEAD 2> /dev/null) || ref="➦ $(git show-ref --head -s --abbrev |head -n1 2> /dev/null)"
    if [[ -n $dirty ]]; then
      prompt_segment yellow black
    else
      prompt_segment green black
    fi
    echo -n "${ref/refs\/heads\//⭠ }$dirty"
  fi
}

# Dir: current working directory
prompt_dir_old() {
  prompt_segment blue black '%~'
}

prompt_dir() {
  prompt_segment blue black "`get_short_path`"
}

# Status:
# - was there an error
# - am I root
# - are there background jobs?
prompt_status() {
  local symbols
  symbols=()
  [[ $RETVAL -ne 0 ]] && symbols+="%{%F{red}%}✘"
  [[ $UID -eq 0 ]] && symbols+="%{%F{yellow}%}⚡"
  [[ $(jobs -l | wc -l) -gt 0 ]] && symbols+="%{%F{cyan}%}⚙"

  [[ -n "$symbols" ]] && prompt_segment black black "$symbols"
}

### current time
### function added by digitalformula
prompt_time() {
  prompt_segment white black "`date +%H:%M:%S`"
}

### current hostname
### function added by digitalformula
prompt_hostname() {
  hostname=`hostname`
  prompt_segment green black "$hostname"
}

## Main prompt (Old version, includes currect directory)
build_prompt_old() {
  RETVAL=$?
  prompt_status
  prompt_context
  prompt_dir
  prompt_git
  prompt_end
}

build_prompt() {
  RETVAL=$?
  prompt_status
  prompt_time
  prompt_hostname
  prompt_dir
  prompt_git
  prompt_end
}

function prompt_agnoster_setup {
  setopt LOCAL_OPTIONS
  unsetopt XTRACE KSH_ARRAYS
  prompt_opts=(cr percent subst)

  # Load required functions.
  autoload -Uz add-zsh-hook

  # Add hook for calling git-info before each command.
  add-zsh-hook precmd prompt_agnoster_precmd

  zstyle ':prezto:module:editor' completing '%B%F{red}...%f%b'
  zstyle ':prezto:module:editor:keymap:primary' overwrite ' %F{red}♺%f'
  zstyle ':prezto:module:editor:keymap' primary ' %B%F{red}❯%F{yellow}❯%F{green}❯%f%b'
  zstyle ':prezto:module:editor:keymap' alternate ' %B%F{green}❮%F{yellow}❮%F{red}❮%f%b'
  zstyle ':prezto:module:git' action ':%%B%F{yellow}%s%f%%b'
  zstyle ':prezto:module:git' added ' %%B%F{green}✚%f%%b'
  zstyle ':prezto:module:git' ahead ' %%B%F{yellow}⬆%f%%b'
  zstyle ':prezto:module:git' behind ' %%B%F{yellow}⬇%f%%b'
  zstyle ':prezto:module:git' branch ':%F{green}%b%f'
  zstyle ':prezto:module:git' commit ':%F{green}%.7c%f'
  zstyle ':prezto:module:git' deleted ' %%B%F{red}✖%f%%b'
  zstyle ':prezto:module:git' modified ' %%B%F{blue}✱%f%%b'
  zstyle ':prezto:module:git' position ':%F{green}%p%f'
  zstyle ':prezto:module:git' renamed ' %%B%F{magenta}➜%f%%b'
  zstyle ':prezto:module:git' stashed ' %%B%F{cyan}✭%f%%b'
  zstyle ':prezto:module:git' unmerged ' %%B%F{yellow}═%f%%b'
  zstyle ':prezto:module:git' untracked ' %%B%F{white}◼%f%%b'
  zstyle ':prezto:module:git' info \
    'prompt'  ' %F{blue}git%f$(coalesce "%b" "%p" "%c")%s' \
    'rprompt' '%A%B%S%a%d%m%r%U%u'
    
# Original theme's comments left intact:

# vim:ft=zsh ts=2 sw=2 sts=2
#
# agnoster's Theme - https://gist.github.com/3712874
# A Powerline-inspired theme for ZSH
#
# # README
#
# In order for this theme to render correctly, you will need a
# [Powerline-patched font](https://gist.github.com/1595572).
#
# In addition, I recommend the
# [Solarized theme](https://github.com/altercation/solarized/) and, if you're
# using it on Mac OS X, [iTerm 2](http://www.iterm2.com/) over Terminal.app -
# it has significantly better color fidelity.
#
# # Goals
#
# The aim of this theme is to only show you *relevant* information. Like most
# prompts, it will only show git information when in a git working directory.
# However, it goes a step further: everything from the current user and
# hostname to whether the last call exited with an error to whether background
# jobs are running in this shell will all be displayed automatically when
# appropriate.

### Segment drawing
# A few utility functions to make it easy and re-usable to draw segmented prompts

CURRENT_BG='NONE'
SEGMENT_SEPARATOR='⮀'

PROMPT='%{%f%b%k%}$(build_prompt) '

}

prompt_agnoster_setup "$@"

