DESTDIR =
PREFIX = /usr/local
BINDIR = $(PREFIX)/bin
SHAREDIR = $(PREFIX)/share/imapfilter
MANDIR = $(PREFIX)/man

CFLAGS = -Wall -O -DMAKEFILE_SHAREDIR='"$(SHAREDIR)"'
LDFLAGS =
LIBS = -lm -llua -lpcre -lssl -lcrypto

MAN1 = imapfilter.1
MAN5 = imapfilter_config.5

LUA = common.lua set.lua regex.lua account.lua mailbox.lua message.lua \
      options.lua auxiliary.lua deprecated.lua

BIN = imapfilter
OBJ = auth.o buffer.o cert.o core.o file.o imap.o imapfilter.o list.o log.o \
      lua.o memory.o misc.o namespace.o pcre.o regexp.o request.o response.o \
      session.o signal.o socket.o system.o

all: $(BIN)

nossl:
	$(MAKE) $(BIN) CFLAGS="-Wall -O -DMAKEFILE_SHAREDIR='\"$(SHAREDIR)\"' \
		-DNO_SSLTLS -DNO_CRAMMD5" LIBS="-lm -llua -lpcre"

$(BIN): $(OBJ)
	$(CC) -o $(BIN) $(LDFLAGS) $(OBJ) $(LIBS)

$(OBJ): imapfilter.h
auth.o: session.h
buffer.o: buffer.h 
cert.o: pathnames.h session.h
file.o: pathnames.h
imap.o: buffer.h session.h
imapfilter.o: buffer.h list.h pathnames.h regexp.h session.h version.h
list.o: list.h
log.o: list.h pathnames.h session.h
lua.o: pathnames.h
namespace.o: buffer.h 
regexp.o: regexp.h
request.o: buffer.h session.h
response.o: buffer.h regexp.h session.h
session.o: list.h session.h
socket.o: session.h

install: $(BIN)
	mkdir -p $(DESTDIR)$(BINDIR) && \
	cp -f $(BIN) $(DESTDIR)$(BINDIR) && \
	chmod 0755 $(DESTDIR)$(BINDIR)/$(BIN)
	mkdir -p $(DESTDIR)$(SHAREDIR) && \
	cp -f $(LUA) $(DESTDIR)$(SHAREDIR) && \
	chmod 0644 $(DESTDIR)$(SHAREDIR)/$(LUA)
	mkdir -p $(DESTDIR)$(MANDIR)/man1 && \
	cp -f ../doc/$(MAN1) $(DESTDIR)$(MANDIR)/man1 && \
	chmod 0644 $(DESTDIR)$(MANDIR)/man1/$(MAN1)
	mkdir -p $(DESTDIR)$(MANDIR)/man5 && \
	cp -f ../doc/$(MAN5) $(DESTDIR)$(MANDIR)/man5 && \
	chmod 0644 $(DESTDIR)$(MANDIR)/man5/$(MAN5)

uninstall:
	rm -f $(DESTDIR)$(BINDIR)/$(BIN)
	cd $(DESTDIR)$(SHAREDIR) && rm -f $(LUA)
	rm -f $(DESTDIR)$(MANDIR)/man1/$(MAN1)
	rm -f $(DESTDIR)$(MANDIR)/man5/$(MAN5)

clean:
	rm -f $(OBJ) $(BIN) *~
