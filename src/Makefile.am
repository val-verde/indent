AUTOMAKE_OPTIONS = 1.2 gnits

bin_PROGRAMS = indent
indent_SOURCES = indent.c args.c backup.c comments.c lexi.c parse.c globs.c code_io.c output.c utils.c handletoken.c wildexp.c
noinst_HEADERS = indent.h args.h backup.h comments.h lexi.h parse.h globs.h code_io.h output.h utils.h sys.h handletoken.h
indent_LDADD =  $(INTLLIBS)
EXTRA_DIST = @PACKAGE@.gperf @PACKAGE@-cc.gperf wildexp.c gperf.c gperf-cc.c \
	     @PACKAGE@.dsp \
	     config.h.vc++


BUILT_SOURCES = gperf.c gperf-cc.c
AM_CPPFLAGS = -I. -I$(srcdir) -I$(top_srcdir)/intl
DEFS = @DEFS@ -DLOCALEDIR=\"${localedir}\"
LIBS = @LIBINTL@ @LIBS@


MAINTAINERCLEANFILES= Makefile.in libgettext.h

AM_CFLAGS  = -Wall -W -Wredundant-decls -Wshadow -Wstrict-prototypes
AM_CFLAGS += -Wmissing-prototypes -Wnested-externs -Wmissing-declarations
AM_CFLAGS += -Wcomment -Wbad-function-cast -Wcast-align

## Use -g when in maintainer mode
if MAINTAINER_MODE
AM_CFLAGS += -g -Wextra -Werror -fstack-protector-strong -fPIE -pie
AM_CFLAGS += -fsanitize=address -fsanitize=undefined -fno-omit-frame-pointer
AM_CPPFLAGS += -D_FORTIFY_SOURCE=2
AM_LDFLAGS = -Wl,-z,relro -Wl,-z,now -static-libasan -static-libubsan
endif

gperf.c: indent.gperf
	@gperf -D -c -l -p -t -T -g -j1 -o -K rwd\
	       -L ANSI-C -N is_reserved indent.gperf > gperf.c
	@sed -i -e 's/index/idx/g'\
	        -e 's/ifdef __GNUC__/if defined(__GNUC__) \&\& \!defined(__clang__)/'\
	         gperf.c

gperf-cc.c: indent-cc.gperf
	@gperf -D -c -l -p -t -T -g -j1 -o -K rwd\
	       -L ANSI-C -N is_reserved_cc -H hash_cc\
	       indent-cc.gperf > gperf-cc.c
	@sed -i -e 's/index/idx/g'\
	        -e 's/ifdef __GNUC__/if defined(__GNUC__) \&\& \!defined(__clang__)/'\
	         gperf-cc.c

if MAINTAINER_MODE
config.h.vc++ @PACKAGE@.lsm :%: Makefile %.in
	sed -e 's%!VERSION!%@VERSION@%g' $@.in > $@
endif
