% VIMPAGER(1) vimpager user manual
% Rafael Kitover <rkitover@gmail.com>
% August 4, 2014

# NAME

vimpager - less.sh replacement

# SYNOPSIS

vimpager 'some file'

&#35; or (this won't always syntax highlight as well)

cat 'some file' | vimpager

# RUN-TIME DEPENDENCIES

* vim
* sharutils or some uudecode (optional)

# BUILD DEPENDENCIES

* sharutils or some uuencode
* pandoc

# INSTALL

```bash
git clone git://github.com/rkitover/vimpager
cd vimpager
sudo make install
```

In your ~/.bashrc add the following:

```bash
export PAGER=/usr/local/bin/vimpager
alias less=$PAGER
alias zless=$PAGER
```

# DESCRIPTION
A slightly more sophisticated replacement for less.sh that also supports being
set as the PAGER environment variable. Displays man pages, perldocs and python
documentation properly. Works on Linux, Solaris, FreeBSD, NetBSD, OpenBSD, OSX,
Cygwin and msys. Should work on most other systems as well.

On GitHub: <http://github.com/rkitover/vimpager>

To use a different vimrc with vimpager, put your settings into a ~/.vimpagerrc
or ~/.vim/vimpagerrc or a file pointed to by the VIMPAGER_RC environment
variable.

You can also have a global config file for all users in /etc/vimpagerrc, users
can override it by creating a ~/.vimpagerrc or a ~/.vim/vimpagerrc.

To disable loading plugins, put "set noloadplugins" into a vimpagerrc
file.

You can also switch on the "vimpager" variable in your vimrc to set alternate
settings for vimpager.

Put the following into your .vimrc/vimpagerrc if you want to use gvim/MacVim
for your pager window:

```vim
let vimpager_use_gvim = 1
```

To turn off the feature of passing through text that is smaller than the
terminal height use this:

```vim
let vimpager_passthrough = 0
```

See "PASSTHROUGH MODE" further down.

To start vim with -X (no x11 connection, a bit faster startup) put the following
into your .vimrc/vimpagerrc:

```vim
let vimpager_disable_x11 = 1
```

The scroll offset (:help scrolloff), may be specified by placing the 
following into your .vimrc/vimpagerrc (default = 5, disable = 0):

```vim
let vimpager_scrolloff = 5
```

The process tree of vimpager is available in the "vimpager_ptree" variable, an
example usage is as follows:

```vim
if exists("vimpager")
  if exists("vimpager_ptree") && vimpager_ptree[-2] == 'wman'
    set ft=man
  endif
endif
```

To disable the use of AnsiEsc.vim to display ANSI colors in the source,
set:

```vim
let vimpager_disable_ansiesc = 1
```

see the section "ANSI ESCAPE SEQUENCES AND OVERSTRIKES" for more
details.

# COMMAND LINE OPTIONS

## + | +G

Start at the end of the file, just like less.

## -c cmd

Run a vim command after opening the file.

# ANSI ESCAPE SEQUENCES AND OVERSTRIKES

If your source is using ANSI escape codes, the AnsiEsc plugin will be
used to show them, rather than the normal vim highlighting, however read
the caveats below. If this is not possible, they will be stripped out
and normal vim highlighting will be used instead.

Overstrikes such as in man pages will always be removed.

vimpager bundles the
[AnsiEsc](http://www.vim.org/scripts/script.php?script_id=4979)
plugin (it is expanded at runtime,
there is nothing you have to do to enable it.)

However, your vim must have been compiled with the 'conceal' feature
enabled. To check, try

```vim
:echo has("conceal")
```

if the result is '1' you have conceal, if it's '0' you do not, and the
AnsiEsc plugin will not be enabled.

If you're on a Mac, the system vim does not enable this feature, install
vim from Homebrew.

To disable the use of AnsiEsc.vim, set:

```vim
let vimpager_disable_ansiesc = 1
```

in your .vimrc.

# PASSTHROUGH MODE

If the text sent to the pager is smaller than the terminal window, then
it will be displayed without vim as text. If it has ansi codes, they
will be preserved, depending on your "vimpager_disable_ansiesc" setting,
otherwise the text will be highlighted with vimcat.

You can turn this off by putting

```vim
let vimpager_enable_passthrough = 0
```

Passthrough mode requires a POSIX shell with arithmetic expansion, if
there is one on your system and it is not detected please submit an
issue with the path and your OS version.

# CYGWIN/MSYS/MSYS2 NOTES

vimpager works correctly with the native Windows gvim, just put it in
your PATH and set the vimpager_use_gvim option as described above.

# LICENSE AND COPYRIGHT

Copyright (c) 2014, Rafael Kitover <rkitover@gmail.com> and contributors
(see the list of CONTRIBUTORS at the bottom of the 'vimpager' file.)

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

Redistributions of source code must retain the above copyright
notice, this list of conditions and the following disclaimer.

Redistributions in binary form must reproduce the above copyright
notice, this list of conditions and the following disclaimer in the
documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDER ``AS IS'' AND ANY
EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED
WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE
DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER BE LIABLE FOR ANY
DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES
(INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES;
LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND
ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
(INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS
SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
