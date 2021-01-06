# Ocaml-On-Pixelbook
Run OCaml on a Google Pixelbook's Debian VM (Crostini). This probably works on Vanilla Ubuntu as well

# Pre Install Chromebook Stuff
Check to make sure back button is working in chrome  
Remap keys  
* Caps -> Control  
* Control -> App drawer
* Update touchpad settings (prefer Austrailian [apple calls "natural"]

# Linux/Crostini:
enable in settings  
* In settings just search “Linux”

# Install OCaml stuff:
https://dev.realworldocaml.org/install.html

# RWO install [Tested on Debian 10 (Buster)]
~~~~
$ sudo apt-get install curl build-essential m4 zlib1g-dev libssl-dev ocaml ocaml-native-compilers opam
$ opam init
$ opam switch create 4.10.0
$ opam switch # This shows compiler versions
$ opam switch 4.10.0
$ opam install core utop
$ opam depext conf-pkg-config.1.3
$ opam depext conf-gmp.3
$ opam depext conf-libpcre.1
$ opam install async yojson core_extended core_bench cohttp async_graphics cryptokit menhir
# done with realworld install
~~~~

# sagarmomin install some tools
~~~~
$ opam install patdiff sexp ocamlformat 
~~~~

# Housekeeping
Ocaml Code directory structure I  like to put stuff in  
* workspaces
  * app
  * lib
  * external
  
In external I put all my opam pin’d libraries  

# Generally useful Linux stuff and other tools
~~~~
$ sudo apt-get install curl fdisk fzf grep gzip inetutils-ping inotify-tools jq man net-tools nmap python3 ripgrep strace x11-apps xclip xorg

~~~~

# Install VIM that has python support. I prefer vim-nox
~~~~
$ sudo apt-get install vim-nox
# Copy over my vim-config 
$ git clone https://github.com/SagarMomin/vim-config.git ~/.vim
~~~~

# Install opinionated bashrc and tmux configs in order to make sure everything works
~~~~
# If you don't want to overwrite your bashrc, you'll have to manually 
# look through: ~/.vim/system-configs/setup/
$ bash ~/.vim/system-configs/smash-all-my-configs.sh
~~~~

# Making OCaml + vim = :D
~~~~
$ opam install merlin ocp-indent
$ opam install user-setup # Not strictly necessary
$ opam init
~~~~

# Make sure Core is available in utop
~~~~
$ cat << EOM >> ~/.ocamlinit
#use "topfind";;
#thread;;
#require "core.top";;
EOM
~~~~

# Get vim to successfull load via plugin manager
~~~~
# Make sure VIM is installed. It will error the first time
$ bash ~/.vim/bin/install.sh

# Deprecated: Manually start vim, then restart vim
# $ vim
# $ :PlugInstall # in vim
~~~~

# Git stuff
in ~.gitconfig: (If you do this, don't use user/password. Use a token or SSH keys)
~~~~
---------------------------------------------------------------------------------------
[credential]
  helper = store
[diff]
  external = /home/sagarmomin/.opam/4.10.0/bin/patdiff-git-wrapper
[user]
  email = {EMAIL}
  name = Sagar
---------------------------------------------------------------------------------------
~~~~

Patdiff script: [~/.local/bin/patdiff-git.sh] (don't forget to chmod +x)
~~~~
---------------------------------------------------------------------------------------
#!/usr/bin/env bash

patdiff "$2" "$5" | cat
---------------------------------------------------------------------------------------
~~~~
