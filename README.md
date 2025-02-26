# Notmuch Addressbook manager for vim

## Requirements

- [`notmuch`](https://notmuchmail.org/) with Python3 bindings

## Installation

### Standalone

If you do not want to use the Vundle/Vimscript file, you can install the module as follows:

```sh
python3 setup.py install
```

or using:

```sh
pip3 install notmuch_abook
```

### Vundle/Vimscript

Use [`Vundle`](https://github.com/VundleVim/Vundle.vim) to install this script, add to `${HOME}/.vimrc`:

```vim
Plugin 'johannesheinz/notmuch-abook'
```

for convenience, you can create a symlink to your bin directory:

```sh
ln -s ${HOME}/.vim/bundle/notmuch-abook/pythonx/notmuch_abook.py /usr/local/bin/notmuch-abook
```

## Configuration

Open notmuch configuration file (default is `${HOME}/.notmuch-config`) and add:

```ini
[addressbook]
path=/home/USER/.notmuch-abook.db
backend=sqlite3
```

where USER is your username (or at any other place).

The default notmuch query string is:

```
NOT tag:junk AND NOT folder:drafts AND NOT tag:deleted
```

If you prefer something else, you can specify it in notmuch configuration file:

```ini
[addressbook]
path=/home/USER/.notmuch-abook.db
backend=sqlite3
query=folder:Inbox OR folder:Sent
```

If you use a non-default notmuch configuration file, you should set the `NOTMUCH_CONFIG` environment variable (see notmuch man page). This can even be done inside the `${HOME}/.vimrc` file, with:

```vim
let $NOTMUCH_CONFIG = expand("~/.notmuch-config-custom")
```

In your favorite mail filter program, add a rule such as (for `procmail`), so all new mail will be parsed:

```
:0 Wh
| python $HOME/.vim/bundle/notmuch-abook/pythonx/notmuch_abook.py update
```

If you can't use `procmail` (e.g. if you are using `offlineimap`) then you could put the following few lines at the start of the [post-new hook](http://notmuchmail.org/manpages/notmuch-hooks-5/) (**before** you remove the new tag). 
Also note this is shell syntax, so you'll have to adapt if your
hook is in another language.

```sh
# First update notmuch-abook
for file in $(notmuch search --output=files tag:new)
do
    cat "${file}" | /usr/local/bin/notmuch-abook update
done
```

## Usage

For the first time, you shall launch the script as follows to create the addresses database (it takes ~20 seconds for 10000 mails):

```sh
python3 ${HOME}/.vim/bundle/notmuch-abook/pythonx/notmuch_abook.py create
```

or

```sh
/usr/local/bin/notmuch-abook create
```

and then, to lookup an address, either you use the vimscript to complete (`<c-x><c-u>`) the name in a header field, or you can call it from commandline:

```sh
python ${HOME}/.vim/bundle/notmuch-abook/pythonx/notmuch_abook.py lookup "John Doe"
```

or

```sh
/usr/local/bin/notmuch-abook lookup John
```

the script will match any word beginning with the pattern in the name and address parts of the entry.

### Contributors

- Current Maintainer of this Fork: [Johannes Heinz](https://github.com/johannesheinz)
- Previous Maintainer: [Bernard _Guyzmo_ Pratz](https://github.com/guyzmo)
- Contributors:
  - [Lucas Hoffmann](https://github.com/lucc)
  - [Hamish Downer](https://github.com/foobacca)
  - [Tomas Tomecek](https://github.com/TomasTomecek)
  - [David Edmondson](https://github.com/dme)
  - [Jesse Rosenthal](https://github.com/jkr)

### License

(c) 2013, Bernard Guyzmo Pratz, guyzmo at m0g dot net

Even though it is a WTFPL license, if you do improve that code, it's great, but if you don't tell me about that, you're just a moron. And if you like that code, you have the right to buy me a beer, thank me, or [flattr](http://flattr.com/profile/guyzmo)/[gittip](http://gittip.com/guyzmo) me.

```
DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
Version 2, December 2004

Copyright (C) 2004 Sam Hocevar <sam@hocevar.net>

Everyone is permitted to copy and distribute verbatim or modified
copies of this license document, and changing it is allowed as long as the name is changed.

DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE
TERMS AND CONDITIONS FOR COPYING, DISTRIBUTION AND MODIFICATION

0. You just DO WHAT THE FUCK YOU WANT TO.
```

