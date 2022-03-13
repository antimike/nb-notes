# GitHub - yuki-yano/zeno.zsh: zsh fuzzy completion and utility plugin with Deno. (github.com)

<https://github.com/yuki-yano/zeno.zsh>

## Description

zsh fuzzy completion and utility plugin with Deno. - GitHub - yuki-yano/zeno.zsh: zsh fuzzy completion and utility plugin with Deno.

## Comment

FZF-powered zsh plugin for deno (modern JS runtime)

## Tags

#deno #to-try #fzf #terminal

## Content

GitHub - yuki-yano/zeno.zsh: zsh fuzzy completion and utility plugin with Deno. {#github---yuki-yanozeno.zsh-zsh-fuzzy-completion-and-utility-plugin-with-deno. .reader-title}
===============================================================================

------------------------------------------------------------------------

zeno.zsh {#zeno.zsh dir="auto"}
--------

zsh fuzzy completion and utility plugin with [Deno](https://deno.land/).

**WARNING**: This project is in beta stage and WIP README.

Features {#features dir="auto"}
--------

-   Insert snippet and abbrev snippet
-   Completion with fzf
    -   Builtin git completion
    -   User defined completion
-   ZLE utilities

Demo {#demo dir="auto"}
----

### Abbrev snippet {#abbrev-snippet dir="auto"}

[![zeno](https://user-images.githubusercontent.com/5423775/119225771-e0dfda80-bb40-11eb-8001-f5b575e29707.gif "zeno")](https://user-images.githubusercontent.com/5423775/119225771-e0dfda80-bb40-11eb-8001-f5b575e29707.gif)

### Completion with fzf {#completion-with-fzf dir="auto"}

[![zeno](https://user-images.githubusercontent.com/5423775/119226132-aaa35a80-bb42-11eb-9b90-1071fce1fc7d.gif "zeno")](https://user-images.githubusercontent.com/5423775/119226132-aaa35a80-bb42-11eb-9b90-1071fce1fc7d.gif)

Requirements {#requirements dir="auto"}
------------

-   [Deno](https://deno.land/) latest
-   [fzf](https://github.com/junegunn/fzf)

Installation {#installation dir="auto"}
------------

### zinit {#zinit dir="auto"}

    zinit ice lucid depth"1" blockf
    zinit light yuki-yano/zeno.zsh

### git clone {#git-clone dir="auto"}

    $ git clone https://github.com/yuki-yano/zeno.zsh.git
    $ echo "source /path/to/dir/zeno.zsh" >> ~/.zshrc

Usage {#usage dir="auto"}
-----

### Abbrev snippet {#abbrev-snippet-1 dir="auto"}

Require [user configuration file](#user-configuration-file-example)

    $ gs<Space>

    Insert
    $ git status --short --branch

    $ gs<Enter>

    Execute
    $ git status --short --branch

### Completion {#completion dir="auto"}

    $ git add <Tab>
    Git Add Files> ...

### Insert snippet {#insert-snippet dir="auto"}

Use zeno-insert-snippet zle

### Search history {#search-history dir="auto"}

Use zeno-history-completion zle

### Change ghq managed repository {#change-ghq-managed-repository dir="auto"}

Use zeno-ghq-cd zle

Configuration example {#configuration-example dir="auto"}
---------------------

### Completion and abbrev snippet {#completion-and-abbrev-snippet dir="auto"}

    # if defined load the configuration file from there
    # export ZENO_HOME=~/.config/zeno

    # if disable deno cache command when plugin loaded
    # export ZENO_DISABLE_EXECUTE_CACHE_COMMAND=1

    # if enable fzf-tmux
    # export ZENO_ENABLE_FZF_TMUX=1

    # if setting fzf-tmux options
    # export ZENO_FZF_TMUX_OPTIONS="-p"

    # Experimental: Use UNIX Domain Socket
    export ZENO_ENABLE_SOCK=1

    # if disable builtin completion
    # export ZENO_DISABLE_BUILTIN_COMPLETION=1

    # default
    export ZENO_GIT_CAT="cat"
    # git file preview with color
    # export ZENO_GIT_CAT="bat --color=always"

    # default
    export ZENO_GIT_TREE="tree"
    # git folder preview with color
    # export ZENO_GIT_TREE="exa --tree"

    if [[ -n $ZENO_LOADED ]]; then
      bindkey ' '  zeno-auto-snippet

      # fallback if snippet not matched (default: self-insert)
      # export ZENO_AUTO_SNIPPET_FALLBACK=self-insert

      # if you use zsh's incremental search
      # bindkey -M isearch ' ' self-insert

      bindkey '^m' zeno-auto-snippet-and-accept-line

      bindkey '^i' zeno-completion

      # fallback if completion not matched
      # (default: fzf-completion if exists; otherwise expand-or-complete)
      # export ZENO_COMPLETION_FALLBACK=expand-or-complete
    fi

### ZLE widget {#zle-widget dir="auto"}

    if [[ -n $ZENO_LOADED ]]; then
      bindkey '^r'   zeno-history-selection
      bindkey '^x^s' zeno-insert-snippet
      bindkey '^x^f' zeno-ghq-cd
    fi

Builtin completion {#builtin-completion dir="auto"}
------------------

-   git
    -   add
    -   diff
    -   diff file
    -   checkout
    -   checkout file
    -   switch
    -   reset
    -   reset file
    -   restore
    -   fixup and squash commit
    -   rebase
    -   merge

See: <https://github.com/yuki-yano/zeno.zsh/blob/main/src/completion/source/git.ts>

User configuration file {#user-configuration-file dir="auto"}
-----------------------

The configuration file is searched from the following.

-   `$ZENO_HOME/config.yml`
-   `$XDG_CONFIG_HOME/zeno/config.yml` or `~/.config/zeno/config.yml`
-   Find `.../zeno/config.yml` from each in `$XDG_CONFIG_DIRS`

### Example {#example dir="auto"}

    $ touch ~/.config/zeno/config.yml

and

    snippets:
      # snippet and keyword abbrev
      - name: git status
        keyword: gs
        snippet: git status --short --branch
      # snippet with placeholder
      - name: git commit message
        keyword: gcim
        snippet: git commit -m '{{commit_message}}'
      - name: "null"
        keyword: "null"
        snippet: ">/dev/null 2>&1"
        # auto expand condition
        # If not defined, it is only valid at the beginning of a line.
        context:
          # buffer: '' 
          lbuffer: '.+\s'
          # rbuffer: ''
      - name: branch
        keyword: B
        snippet: git symbolic-ref --short HEAD
        context:
          lbuffer: '^git\s+checkout\s+'
        evaluate: true # eval snippet

    completions:
      - name: kill
        patterns: 
          - "^kill( -9)? $"
        sourceCommand: "ps -ef | sed 1d"
        options:
          --multi: true
          --prompt: "'Kill Process> '"
        callback: "awk '{print $2}'"

Related project {#related-project dir="auto"}
---------------

-   [fzf](https://github.com/junegunn/fzf)
-   [fzf-tab](https://github.com/Aloxaf/fzf-tab)
-   [fzf-zsh-completions](https://github.com/chitoku-k/fzf-zsh-completions)
-   [pmy](https://github.com/relastle/pmy/)
-   [zabrze](https://github.com/Ryooooooga/zabrze)
