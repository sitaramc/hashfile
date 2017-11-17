# hashfile

`hashfile` is a low-volume, low-load low-learning curve, low-impedance,
key-value store for use by shell scripts.

I wrote it because for most basic needs, my earlier `hashlite` project was too
big.  I also wanted something whose backend could be a pure perl module
(because reasons!)

See the appendix below for more on when you should use which of these two, if
you're not sure.  (The **irony** that "hash**lite**" is the more heavy-weight
one is not lost on me.  Of course, the "lite" in that name comes from the fact
that its backing store is "SQLite", so I guess it *is* lighter than if I were
using some other database!)

`hashfile` has decent help; just run it without arguments.  It's quite
self-explanatory.  At present it looks like this:

        Usage:
            hashfile new   $file                # create new hashfile
            hashfile set   $file $k [$v]        # set value or delete key
            hashfile get   $file $k             # print value

            hashfile grep  $file -k|-v $pattern # grep keys or values, print matching keys

            hashfile clear $file $pattern       # delete all matching keys, print count
            hashfile dump  $file                # dump one $k<TAB>$v per line, then print count

            hashfile lock  $file $key $$        # (advisory) exclusive lock on $key, no wait
            hashfile unlock $file $key $$       # release lock

        Notes:

        1.  If a relative path is given, it's considered relative to $HASHFILE_DIR if
            defined, else current directory.

        2.  For anything described as 'print' above, you can use '-n' or '-q' after
            the command to suppress the trailing newline, or the output itself.  (For
            the 'dump' command, this only applies to the trailing 'count'; the actual
            dump cannot be suppressed).

        3.  The exit code is set (0 for 'found', 1 for 'not found') when appropriate.
            Combined with '-q', this can be very useful in shell scripts.

        4.  The lock can be *easily* defeated by a bug in your code; simply delete the
            lock key!  But your programs don't have bugs, right?

# appendix A: DBM::Deep versus SQLite

`hashfile` and `hashlite` have somewhat similar functionality.  The main
differences are:

*   `hashfile` uses `DBM::Deep`, while `hashlite` uses `SQLite`
*   `hashfile` does not have a perl API; it's only for shell use.

DBM::Deep is great for persisting small data sets with light IO loads.  Keep
the DB to less than about 2-3 GB.

For anything larger, or if you have heavy IO, `SQLite` is better -- it is
faster, much more rigorously tested, and takes less space on disk.
