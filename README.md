# simple-tags

## Overview

`simple-tags` (or `stag` for short) is a command line tool to assign tags
to files, and search files with those tags. It has no dependency except
Python 3. Installation is easy: just copy `stag` somewhere in your `PATH`.

Like `git`, a simple `.stags` files is used to manage the tags of a whole
subtree on your filesystem. You can initialize an empty database with
`stag create`.

You can tags a file (or directory) with `stag add file tag1
tag2...`, and remove tags with `stag del file tag1 tags2...`.

You can list tags with `stag ls` (`-a` to show hidden files, `-r` for
recursive, likes `ls`). If you add a filter (`-f` options), only tags
matching the filter will be displayed.

## Filters syntax

* The basic unit of a filter is a tag: `tag`. Wildcard matches are
allowed: `source:http://*` will match `source:http://news.ycombinator.com`
and `source:http://reddit.com` (and `source:http://` too).

* `filter1 & filter2` will match if both `filter1` and `filter2` match.

* `filter1 filter2` is shorthand for `filter1 & filter2`. Radical
laziness FTW.

* `filter1 | filter2` will match if any of `filter1` or `filter2` match.

* `!filter` will match if `filter` do not match.

* `!` takes precedence over `&` and `|`. Precedence between `&` and `|`
is not specified ; `a&b|c` can be either `(a&b)|c` or `a&(b|c)` depending
on random stuff (actually they are left-associative, but no guarantee
it will stay the same). Use parenthesis to resolve the ambiguity.

Example filter: `year:201* photos !embarrassing (vacation | wedding)`.

## Limitations

Tags does not follow files renaming (or file copy, or `tar`, or
`rsync`…, unless you include the database file). Using `xattr` instead
of a database file could have solved that, at the price of being unusable
over NFS/CIFS/sshfs. I didn’t want to pay that price.
