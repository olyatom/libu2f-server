# Copyright (c) 2014 Yubico AB
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are
# met:
#
# # Redistributions of source code must retain the above copyright
# notice, this list of conditions and the following disclaimer.
#
# # Redistributions in binary form must reproduce the above
# copyright notice, this list of conditions and the following
# disclaimer in the documentation and/or other materials provided
# with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR
# A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT
# OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL,
# SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT
# LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE,
# DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY
# THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT
# (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE
# OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

AM_CFLAGS = $(WARN_CFLAGS)
AM_CPPFLAGS = -I$(srcdir)/.. -I$(builddir)/.. -I$(builddir)/../u2f-server

bin_PROGRAMS = u2f-server

u2f_server_SOURCES = u2f-server.c
u2f_server_SOURCES += cmdline.ggo cmdline.c cmdline.h
u2f_server_LDADD = ../u2f-server/libu2f-server.la

cmdline.c cmdline.h: cmdline.ggo Makefile.am
	gengetopt --no-handle-help --input $^

BUILT_SOURCES = cmdline.c cmdline.h
MAINTAINERCLEANFILES = $(BUILT_SOURCES)

dist_man_MANS = $(top_srcdir)/man/u2f-server.1
DISTCLEANFILES = $(dist_man_MANS)

$(top_srcdir)/man/u2f-server.1: $(srcdir)/u2f-server.c $(srcdir)/cmdline.ggo $(top_srcdir)/configure.ac | $(builddir)/u2f-server$(EXEEXT)
	$(HELP2MAN) \
		--output=$(top_builddir)/man/$@ $(builddir)/u2f-server$(EXEEXT) \
		--name="Yubico Universal 2nd Factor (U2F) Server Tool" \
		--no-info
if ENABLE_H2A
	$(HELP2ADOC) \
		-e $(builddir)/u2f-server$(EXEEXT) \
		--name="Yubico Universal 2nd Factor (U2F) Server Tool" \
		> $(top_builddir)/man/$@.txt
endif

clean-local:
	rm -f $(top_builddir)/man/*.[1-9]
	rm -f $(top_builddir)/man/*.[1-9].txt

if ENABLE_COV
AM_CFLAGS += --coverage
AM_LDFLAGS = --coverage
endif
