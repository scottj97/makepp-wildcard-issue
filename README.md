Makepp compatibility issue
==========================

([Asked about
this](https://sourceforge.net/p/makepp/discussion/142850/thread/f16d3aa3d1/)
on the official makepp help forum.)

This Makefile is based on the system described in [Auto Dependency
Generation](http://make.mad-scientist.net/papers/advanced-auto-dependency-generation/). It
is compatible with GNU Make but when I run it with makepp (2.0.99.2) I
get this failure:

```
$ perl ~/makepp/makepp-2.0.99.2/makepp
makepp: Loading makefile `/home/sjohnson/makepp/experiment/Makefile'
makepp: error: There is no rule to build inexistent /home/sjohnson/makepp/experiment/.deps/hello.d
makepp: 0 files updated and 1 phony target built
/home/sjohnson/makepp/experiment/Makefile:20: error: include statement raised this error:
can't read makefile /home/sjohnson/makepp/experiment/.deps/hello.d--No such file or directory
```

I don't understand why the include statement is attempting to read
that file. I understand that `$(wildcard)` works differently than GNU
Make, and includes files that don't exist but which makepp can
build. But here makepp cannot build hello.d, so why is it included in
the `$(wildcard)`?

Second issue
------------

If I use GNU Make to create `hello.o` and `.deps/hello.d`, then
attempt an incremental build using makepp:

```
$ perl ~/makepp/makepp-2.0.99.2/makepp
makepp: Loading makefile `/home/sjohnson/makepp/experiment/Makefile'
makepp: error: No rule to make `/home/sjohnson/makepp/experiment/|'
makepp: error: Failed to build target `/home/sjohnson/makepp/experiment/|' [-1]
makepp: 0 files updated and 1 target failed
```

Apparently makepp does not understand [order-only
prerequisites](https://www.gnu.org/software/make/manual/make.html#Prerequisite-Types). I
can build a workaround, but shouldn't this incompatibility be
documented in the [makepp
incompatibilities](http://makepp.sourceforge.net/2.1/makepp_incompatibilities.html)
page?
