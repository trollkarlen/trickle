include $(top_srcdir)/Makefile.am.inc

TRICKLEOVERLOADFILES = @LTLIBOBJS@ trickle-overload.lo bwstat.lo trickledu.lo \
                       atomicio.lo schedule.lo

if BUILD_TRICKLED
TRICKLEOVERLOADFILES += xdr.lo
endif

trickle-overload.so: $(TRICKLEOVERLOADFILES)
	$(LIBTOOL) --mode=link $(CC) $(CFLAGS) -o libtmp$@.la                \
		-rpath $(libdir)/trickle $(TRICKLEOVERLOADFILES) $(LIBS)     \
		$(LDFLAGS) -module -avoid-version -nostdlib @LIBDL@
	@rm -f libtmp$@.la tmp$@.so* $@
	@cp .libs/libtmp$@.so $@
	@rm -f .libs/libtmp$@.*

trickleoverloaddir = $(libdir)/trickle
trickleoverload_DATA = trickle-overload.so libtrickle.so

man_MANS = trickle.1

bin_PROGRAMS = trickle

if BUILD_TRICKLED

bin_PROGRAMS += trickled tricklectl
man_MANS += trickled.8 trickled.conf.5

endif


trickle_DEPENDENCIES = @ERRO@ $(LIBOBJS)
trickle_SOURCES = trickle.c util.c getopt.c
trickle_LDADD = @ERRO@ $(LIBOBJS)

if BUILD_TRICKLED
trickled_SOURCES = trickled.c atomicio.c print.c bwstat.c client.c conf.c \
                   util.c cleanup.c getopt.c xdr.c schedule.c
trickled_LDADD = @EVENTLIB@ @TIRPC_LIBS@ $(LIBOBJS)

tricklectl_SOURCES = tricklectl.c trickledu.c atomicio.c xdr.c
tricklectl_LDADD = @ERRO@ @TIRPC_LIBS@ $(LIBOBJS)
endif

AM_CFLAGS = -Werror -Wpedantic -Wall -Icompat @EVENTINC@ @TIRPC_CFLAGS@

overloaddir = $(libdir)

libtrickle.so: trickle-overload.lo atomicio.lo schedule.lo
	$(LIBTOOL) --mode=link $(CC) $(CFLAGS) -o libtmp$@.la \
		-rpath $(libdir)/trickle trickle-overload.lo atomicio.lo $(LIBS) \
		$(LDFLAGS) -module -avoid-version -nostdlib -ldl
	@rm -f libtmp$@.la tmp$@.so* $@
	@cp .libs/libtmp$@.so $@
	@rm -f .libs/libtmp$@.*

CLEANFILES = *.so

EXTRA_DIST = LICENSE README strlcat.c strlcpy.c err.c Makefile.am.inc	\
             compat/sys/queue.h compat/sys/tree.h trickle.1             \
             trickle-overload.c schedule.h

if BUILD_TRICKLED
EXTRA_DIST += trickled.8 trickled.conf.5 trickledu.c trickledu.h print.h cleanup.h conf.h message.h bwstat.h client.h xdr.h
endif

ACLOCAL_AMFLAGS = -I m4
