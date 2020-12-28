---
date: 2008-01-25 05:25
tags: Mac Darwin_Ports bug
title: Darwin Ports issue with "patch"
---

Ever since I've upgraded to Apple Macintosh OS X 10.5 Leopard, I've run into
problems using the Darwinports "port" command to install new software.

The problem is that for some reason the version of GNU "patch" that I have
installed in /usr/bin/patch is version 2.5.8, and it doesn't operate the way
that Darwin ports expects. A typical error message is:

```
---> Applying patches to erlang
Error: Target org.macports.patch returned: shell command " cd "/opt/local/var/macports/build/_opt_local_var_macports_sources_rsync.macports.org_release_ports_lang_erlang/work/erlang-R12B-0" && patch -p0 < '/opt/local/var/macports/sources/rsync.macports.org/release/ports/lang/erlang/files/patch-toolbar.erl'" returned error 2
Command output: Get file lib/toolbar/src/toolbar.erl from Perforce with lock? [y]
Perforce client error:
Connect to server failed; check $P4PORT.
TCP connect to perforce failed.
perforce: host unknown.
patch: **** Can't get file lib/toolbar/src/toolbar.erl from Perforce

Error: Status 1 encountered during processing.
```

The work-around is to define the environment variable
POSIXLY_CORRECT=1 , as in:

```
POSIXLY_CORRECT=1 sudo port install erlang
```

Now, I've done some web searching, and I haven't seen anyone else complaining
about this problem, so perhaps there's something odd about my setup.
