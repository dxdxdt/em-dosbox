AM_CPPFLAGS = -I$(top_srcdir)/include

SUBDIRS = cpu debug dos fpu gui hardware libs ints misc shell platform 

bin_PROGRAMS = dosbox

if HAVE_WINDRES
ico_stuff = winres.rc
endif

.rc.o:
	$(WINDRES) -o $@ $<

dosbox_SOURCES = dosbox.cpp $(ico_stuff)
dosbox_LDADD = cpu/libcpu.a debug/libdebug.a dos/libdos.a fpu/libfpu.a  hardware/libhardware.a gui/libgui.a \
               ints/libints.a misc/libmisc.a shell/libshell.a hardware/mame/libmame.a \
               hardware/serialport/libserial.a libs/gui_tk/libgui_tk.a

EXTRA_DIST = winres.rc dosbox.ico

if EMSCRIPTEN_BUILD
dosbox_LDFLAGS=-s TOTAL_MEMORY=134217728 -s ALLOW_MEMORY_GROWTH=0 -s FORCE_FILESYSTEM
CLEANFILES=dosbox.js dosbox.html.mem dosbox.js.orig.js dosbox.js.tmp.js
if EMTERPRETER
dosbox_LDFLAGS+=-s EMTERPRETIFY=1 -s EMTERPRETIFY_ASYNC=1 -s EMTERPRETIFY_WHITELIST=@$(srcdir)/emterpretify.txt -s FETCH=1
EXTRA_dosbox_DEPENDENCIES=emterpretify.txt
else
if ASYNCIFY
dosbox_LDFLAGS+=-s ASYNCIFY=1 -s FETCH=1 -s ASYNCIFY_ONLY=@$(srcdir)/asyncify.txt
EXTRA_dosbox_DEPENDENCIES=asyncify.txt
endif
endif
if WEBASSEMBLY
dosbox_LDFLAGS+=-s WASM=1
else
dosbox_LDFLAGS+=-s WASM=0
endif
endif

# https://stackoverflow.com/questions/1779984/header-dependency-in-automake
BUILT_SOURCES=version.stamp
version.stamp:
	python $(srcdir)/version.py $(top_builddir)/include/version.h $(top_srcdir) $(CXX)
