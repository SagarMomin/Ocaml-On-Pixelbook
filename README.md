# Ocaml-On-Pixelbook
Run OCaml on a pixel book. This probably works on Vanilla Ubuntu as well

# Pre Install Chromebook Stuff
Install termux and configure  
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

# RWO install
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

# Install sexp
~~~~
# $ git clone https://github.com/janestreet/sexp.git
# $ git checkout 1ea78004d4fcc0fe2148147ca72b32ec722495f0
# Currently the tip of sexp.git isn’t working. Install this version for now, in external
# $ opam pin add sexp sexp
~~~~

# Install js ocamlformat
~~~~
# $ opam pin add ocamlformat -k git https://github.com/janestreet/ocamlformat.git#jane
# The latest #jane removed some formatting I liked. Notably the ;;. 
# Run a specific version of js ocamlformat
$ wget https://github.com/janestreet/ocamlformat/archive/js-1.4-2.zip
$ tar -xvzf js-1.4-2.tar.gz
$ opam pin add ocamlformat ocamlformat-js-1.4-2
~~~~

# Install ppx_deriving_yojson from github. 
~~~~
# Version in Opam doesn’t build. 
# I put in external with all the other pinned libraries
$ git clone https://github.com/ocaml-ppx/ppx_deriving_yojson.git
$ opam pin add ppx_deriving_yojson ppx_deriving_yojson
~~~~

# Generally useful Linux stuff and other tools
~~~~
$ sudo apt-get install xorg strace man nmap inetutils-ping inotify-tools
# tmux 2.7 install (run a newer version) then default ubuntu repo, or build manually
# https://backports.debian.org/Instructions/
$ sudo echo "deb http://ftp.debian.org/debian stretch-backports main" >> /etc/apt/sources.list.d/sources.list
$ apt-get update
$ apt-get -t stretch-backports install tmux

# Install ripgrep
$ curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.8.1/ripgrep_0.8.1_amd64.deb
$ sudo dpkg -i ripgrep_0.8.1_amd64.deb
~~~~

# Install VIM that has python support. I prefer vim-nox
~~~~
$ sudo apt-get install vim-nox
# Copy over my vim-config 
$ git clone https://github.com/SagarMomin/vim-config.git ~/.vim
~~~~

# Making OCaml + vim = :D
~~~~
$ opam install ocamlspot ocp-indent user-setup merlin
$ opam user-setup install # Not strictly necessary
$ opam init
~~~~

# Get vim to successfull load via plugin manager
~~~~
# start vim, then restart vim
$ vim
$ :PlugInstall # in vim
# Before opening vim, we need to fix the python files to be compatible with python3 (in my vim-config)
$ cd ~/.vim && ./fix-python3.sh
# If anything else fails, something is likely missing on the ocaml side
# Check the errors and make sure all the ocaml packages are installed
~~~~

# Git stuff
in ~.gitconfig: (If you do this, don't use user/password. USE A TOKEN)
~~~~
---------------------------------------------------------------------------------------
[credential]
  helper = store
[diff]
  external = /home/sagarmomin/.opam/4.06.1/bin/patdiff-git-wrapper
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


# Issues
* There is an issue with dune 1.2.1. I needed to roll back to 1.1.1
  * Download from: https://opam.ocaml.org/packages/dune/dune.1.1.1/
  * untar as "dune" into workspaces/external
  * opam pin add dune dune
* latest version of sexp relies on code that's not in core yet, which is why I rolled back to the specific git checkout
* The latest Js ocamlformat does not style quite the way I like. I am running a specific version
