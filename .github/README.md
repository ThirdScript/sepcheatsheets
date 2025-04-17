SepCheatSheets
===========
This repository contains community-sourced cheatsheets to be used with
[cheat][] and similar applications.
the old repository is not maintained anymore, I am working on this myself
and I would be very happy if anyone wanted to contribute

Installing
==========
`cheat` has no runtime dependencies. As such, installing it is generally
straightforward. There are a few methods available:

### Install manually
#### Unix-like
On Unix-like systems, you may simply paste the following snippet into your terminal:

```sh
cd /tmp \
  && wget https://github.com/cheat/cheat/releases/download/4.4.2/cheat-linux-amd64.gz \
  && gunzip cheat-linux-amd64.gz \
  && chmod +x cheat-linux-amd64 \
  && sudo mv cheat-linux-amd64 /usr/local/bin/cheat
```

### How to use this ? ###
> cheat -d
```sh
community: /home/sepehr/snap/cheat/common/.config/cheat/cheatsheets/community
personal:  /home/sepehr/snap/cheat/common/.config/cheat/cheatsheets/personal
```
go to either the community or personal folder and add this repository as your remote

#### To change personal cheatsheets to SepCheatsheets
#### If you installed by another way, the personal cheatsheet folder might be in a different path
### Also you need to remove community cheatsheets or do something with it ...
```sh
mkdir -p ~/.config/cheat/cheatsheets/personal
mkdir -p ~/.config/cheat/cheatsheets/community
rm -rf  ~/.config/cheat/cheatsheets/community/*
git clone https://github.com/ThirdScript/sepcheatsheets.git ~/.config/cheat/cheatsheets/personal
```
>>>>>>> 1932f8eb62b083fb2ae2f334d8b4238aa29c7aff

### Format ###
Cheatsheets are plain-text files that begin with an optional "front matter"
header in YAML format. The header may be used to assign "tags" to a sheet, and
to specify the sheet's syntax (`bash`, `python`, `go`, etc).

When possible, cheatsheets should conform to this format:

```sh
---
syntax: bash
tags: [ vcs, development ]
---
# To stage all changes in the current directory:
git add --all

# To commit staged changes:
git commit -m <message>
```

As a guideline, it is preferred to use [docopt][] syntax when specifying
parameter placeholders. In edge-cases where that syntax may cause confusion, it
is permissible to use placeholder values (`foo.txt`, `example.com`, etc.) as
necessary.

### License ###
Cheatsheets are licensed under [Creative Commons CC0 1.0][cc0]. See
[LICENSE.txt][] for the full license text.


[LICENSE.txt]: https://github.com/cheat/cheatsheets/blob/master/.github/LICENSE.txt
[cc0]: https://creativecommons.org/publicdomain/zero/1.0/legalcode
[cheat]:  https://github.com/cheat/cheat
[docopt]: http://docopt.org
