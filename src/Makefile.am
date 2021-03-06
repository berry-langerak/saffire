# Define our programs
bin_PROGRAMS = saffire test

# Set our optimization / debug flags
if DEBUG
  AM_CFLAGS = -ggdb  -O0 -D__DEBUG
  AM_CXXFLAGS = -ggdb -O0 -D__DEBUG
else
  AM_CFLAGS = -O2
  AM_CXXFLAGS = -O2
endif

# Some additional rules for flex/bison file handling
BUILT_SOURCES=compiler/lex.yy.c
AM_LFLAGS= --outfile=compiler/$(LEX_OUTPUT_ROOT).c --header-file=compiler/$(LEX_OUTPUT_ROOT).h
AM_YFLAGS = -d

# parser.tab.h cannot be used before parser.tab.c
compiler/parser.tab.h: compiler/parser.tab.c

# parser.tab.c is created by bison
compiler/parser.tab.c: compiler/saffire.y
	$(YACC) $(AM_YFLAGS) -o $@ $<

# lex.yy.c is created by lex
compiler/lex.yy.c: compiler/saffire.l
	$(LEX) $(AM_LFLAGS) $<

# Cleanup these files on a "make clean"
CLEANFILES = compiler/parser.tab.c compiler/parser.tab.h compiler/lex.yy.c compiler/lex.yy.h \
             compiler/*.o \
             dot/*.o \
             general/*.o \
             interpreter/*.o \
             object/*.o

# Don't install libsaffire. It's only for static linking
noinst_LIBRARIES = libsaffire.a

# Libsaffire is the main core of saffire
libsaffire_a_SOURCES = compiler/parser.tab.c compiler/lex.yy.c \
                  general/svar.c compiler/ast.c dot/dot.c compiler/saffire_compiler.c \
                  interpreter/saffire_interpreter.c general/hashtable.c general/smm.c \
                  general/md5.c general/dll.c \
                  object/object.c object/base.c object/string.c object/numerical.c object/boolean.c \
                  object/null.c object/regex.c

# Link all sources against these libraries
LDADD = libsaffire.a -lpcre

# Define different sources for our targets
saffire_SOURCES = main.c
test_SOURCES = test.c

