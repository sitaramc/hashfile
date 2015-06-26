% hashfile

hashfile is a shell front-end for DBM::Deep.  For most needs, it should
largely replace my earlier "hashlite" project, which uses SQLite as the
backend.

# appendix A: DBM::Deep versus SQLite

DBM::Deep is great for persisting small data sets with light IO loads.  Keep
the DB to less than about 2-3 GB; for anything larger, or if you have heavy
IO, please use SQLite, which is faster, much more rigorously tested, and takes
less space on disk.

The main advantage of using DBM::Deep is lack of impedance mismatch between
the "database" and perl hashes, although in 'hashfile' we don't really hit
that problem (since we don't support structures as values).

Also, it's "pure perl" so it should be easy to install anywhere.  (Not that
SQLite is hard...)
