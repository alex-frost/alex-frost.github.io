---
layout: post
title:  "YADR in Terminator"
tags: [yadr, terminator, debian, jessie]
comments: true
---
{% include JB/setup %}

I'm been using GNU/Linux for almost 10 years and helping friends install it on
their computers.  Recently I tried out Lubuntu for something lightweight.  The
stability hasn't been so good for me and it even crashed a few times.
Seemed time to switch distros, I went for Debian jessie.  When I switch
distros I find one of the most painful things is transferring my settings /
dot files.  I've been pairing with a friend on his Mac and he used YADR so I thought I
should try it out.


### YADR (Yet Another Dot Repo)

YADR is a collection of dot files which adds great
functionality to your shell (if it was Bash then soon you'll be on Zsh), vim and git.
YADR didn't work out of the box so I'm mainly writing this to make the
installation process easier in future and help any one else who has the same issues.

I use Terminator because of its split screen feature and it has all the
options I need.  Except... on my friends' iTerm if you command click on a stack
trace you are taken to the file and line number in your editor of choice: vim!

#### Before installing YADR

Make sure you have ruby working.  I use [rbenv](https://github.com/sstephenson/rbenv) so (copied from the rbenv readme) :

```sh
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
rbenv install 2.1.1
rbenv global 2.1.1
```

also make sure you have a modern version of vim with lua enabled for [neocomplete](https://github.com/Shougo/neocomplete.vim#requirements) to work.  With lua support is included in packages: vim-nox (for command line use), vim-gtk (for graphical vim in KDE environments), vim-gnome (for graphical vim in GNOME) and vim-athena (for graphical vim in [Athena](http://en.wikipedia.org/wiki/Project_Athena)).

#### After installing YADR


* the colours in vim are messed up so I went to Preferences -> Profiles -> Colors
and set the scheme as "Solarized Dark" and the pallet as "Solarized"

* the powerline fonts didnt install with YADR.  I tried following the readme docs
and installing the "for Powerline" fonts and patching fonts but the status bar still had
little squares with numbers inside and not the nice powerline symbols.  Turns out I
I just needed to install them:
`mkdir -p ~/.fonts && cp ~/.yadr/fonts/* "$_" && fc-cache -vf ~/.fonts`

* the default font in gvim was bad, really bad so in "~/.vimrc.after" I added:

```
if has('gui_running')
  set guifont=Monospace\ 10
endif
```

After that YADR was good to go.

#### Extra tweaks

All added to the "~/.vimrc.after" file

* Setting gvim to save when focus is lost on a window (e.g. you click away) stops you
having to type `:w` :
`:au FocusLost * :wa`

* I like to have NERDTree open by default so I also add:
`:au VimEnter * NERDTree | wincmd p`

* To use the linux system clipboard `set clipboard=unnamedplus`

* it is a bit annoying that the Sneak plugin remaps 'S' so I also deleted these lines (295-297)
from ".yadr/vim/bundle/vim-sneak/plugin/sneak.vim"

```diff
-if !hasmapto('<Plug>SneakBackward') && !hasmapto('<Plug>Sneak_S', 'n') && mapcheck('S', 'n') ==# ''
-  nmap S <Plug>Sneak_S
-endif
```

this is definitely not the best way of unmapping something but `unmap S` didn't seem to work of me.


On Debian I also wanted to remap the keyboard so CapsLock and Escape are switched.  I needed to
`apt-get install console-common console-data` then add `XKBOPTIONS="caps:swapescape"` to "/etc/default/keyboard".
To get the configuration to work I `sudo dpkg-reconfigure console-setup` AND restarted (the restart might have been enough).
You might not need the console packages as I was using Xfce.

Hope this has been of some use.
