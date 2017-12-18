# Note:  GNUmakefile is built automatically from GNUmakefile.in
#
# $Id: GNUmakefile.in,v 1.252 2016/07/28 08:16:04 fabiankeil Exp $
#
# Written by and Copyright (C) 2001-2016 members of the
# Privoxy team. https://www.privoxy.org/
#
# Based on the Internet Junkbuster originally written
# by and Copyright (C) 1997 Anonymous Coders and
# Junkbusters Corporation.  http://www.junkbusters.com
#
# This program is free software; you can redistribute it
# and/or modify it under the terms of the GNU General
# Public License as published by the Free Software
# Foundation; either version 2 of the License, or (at
# your option) any later version.
#
# This program is distributed in the hope that it will
# be useful, but WITHOUT ANY WARRANTY; without even the
# implied warranty of MERCHANTABILITY or FITNESS FOR A
# PARTICULAR PURPOSE.  See the GNU General Public
# License for more details.
#
# The GNU General Public License should be included with
# this file.  If not, you can view it at
# http://www.gnu.org/copyleft/gpl.html
# or write to the Free Software Foundation, Inc., 59
# Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#

#############################################################################
# Set make command correctly
#############################################################################


#############################################################################
# Version number (for RPM)
#############################################################################

VERSION_MAJOR = 3
VERSION_MINOR = 0
VERSION_POINT = 26
CODE_STATUS   = stable
VERSION       = $(VERSION_MAJOR).$(VERSION_MINOR).$(VERSION_POINT)
SNAPVERSION   = $(VERSION)-$(shell date "+%Y%m%d")


#############################################################################
# "make install" directories and variables
#############################################################################

#User Group paras
USER         = 
GROUP	   = 

datarootdir  = ${prefix}/share
prefix       = /usr/local
exec_prefix  = ${prefix}
CONF_BASE    = ${prefix}/etc
SBIN_DEST    = ${exec_prefix}/sbin
MAN_DIR      = ${datarootdir}/man
MAN_DEST     = $(MAN_DIR)/man1
MAN_PAGE     = privoxy.1
SHARE_DEST   = ${datarootdir}
DOC_DEST     = $(SHARE_DEST)/doc/privoxy
VAR_DEST     = ${prefix}/var
LOGS_DEST    = $(VAR_DEST)/log/privoxy
PIDS_DEST    = $(VAR_DEST)/run

# if $prefix = /usr/local then the default CONFDEST change from
# CONF_DEST = $(CONF_BASE) to CONF_DEST = $(CONF_BASE)/privoxy
# by the target rule CONF_DEST
#
# also if the $prefix is /usr/local and there is no
# $(SHARE_DEST)/doc, it checks for $prefix/doc and installs there
# instead in this situation
#
# finally if $prefix=/usr/local and VAR_DEST=$prefix/var it
# changes this to /var for storing the logs and pidfile

# used in source dir only, the install goes to $share_dest/doc/privoxy
DOK_WEB = doc/webserver/

# Install usage should be compatible with install-sh.
INSTALL    = /usr/local/bin/ginstall -c
# Binaries
BIN_MODE	 = 0755
# Support files, docs, etc.
RA_MODE   = 0644
# Directory
DIR_MODE   = 0755
# Files daemon writes to.
RWD_MODE   = 0660
INSTALL_P  = -m $(BIN_MODE)
INSTALL_T  = -m $(RA_MODE)
INSTALL_D  = -m $(DIR_MODE) -d
INSTALL_R  = -m $(RWD_MODE)

# install options for superuser install
#INSTALL_S  = -g  -o 

#############################################################################
# Build tools
#############################################################################

PROGRAM    = privoxy
CC         = gcc
ECHO       = echo
GZIP_PROG  = gzip

# id -u is not universal. FIXME: need to set from configure. Breaks on
# Solaris.
#ID         = id -u
ID         = id
LD         = gcc
RM         = rm -f
CP         = cp -f
RMDIR      = rmdir
MKDIR      = ./mkinstalldirs
STRIP_PROG = strip
SED        = sed
GREP       = grep
CAT        = cat
MV         = mv
TAR        = tar
LN         = ln
TOUCH      = touch
KILL       = kill
CHMOD      = chmod
CHOWN      = chown
CHGRP      = chgrp
GROUPS     = groups
W3M_DUMP   = env -u LANG LC_ALL=C  -dump
W3M_DUMP_UTF8 =  -dump
JADECAT    = 
JADEBIN    = jade
DB         = $(JADEBIN) $(JADECAT) -ihtml -t sgml  -D.. -d ldp.dsl\#html
DB2HTML    = false
MAN2HTML   = false
G2H_CMD    = groff -mandoc -Thtml
TARGET_OS  = x86_64-apple-darwin17.3.0
PERL       = perl
DOC_DIR    = doc/source
DOC_TMP    = $(DOC_DIR)/tmp
DOC_STATUS = p-stable
TIDY       = tidy -modify -indent -wrap 78 --tidy-mark no
RSYNC	   = rsync -av -c

# Program to do LF->CRLF
DOSFILTER  = $(PERL) -p -e 's/\n/\r\n/'
CVSROOT    = :pserver:anonymous@ijbswa.cvs.sourceforge.net:/cvsroot/ijbswa
#TMPDIR     := $(shell mktemp -d /tmp/$(PROGRAM).XXXXXX)
# If your SF user name differs from your local one,
# change this to "ssh -l sf-username"
SSH	= ssh
WWW_ROOT = /home/project-web/ijbswa
# SourceForge login name used by the 'sf-shell' target (optional)
SOURCE_FORGE_NAME = ''

#############################################################################
# Setup for make distribution for now.
#############################################################################

TAR_ARCH = /tmp/privoxy-$(VERSION).tar.gz
GEN_DIST_TAR_NAME = privoxy-$(TARGET_OS)-$(VERSION)-$(CODE_STATUS).tar

#############################################################################
# We include these files in our distributions
#############################################################################
CONFIGS = 	config trust default.action match-all.action \
		user.action default.filter user.filter \
		regression-tests.action

# take care that no CVS .cvsignore or other crappy files
# are included here
# and escape every '#' in the find. doh.
CONFIG_FILES = $(CONFIGS) \
		`find templates/ -type f | grep -v "CVS" | grep -v "\.\#" | grep -v ".*~" | grep -v ".cvsignore" | grep -v "TAGS" | sort`

DOC_FILES = AUTHORS LICENSE README ChangeLog INSTALL \
		`find doc/webserver/ -name "*.html" | grep -v "\(webserver\|team\)\/index\.html" | sort` \
		`find doc/webserver/ -name "*.css" | sort` \
                $(MAN_PAGE)

#############################################################################
# Filenames and libraries
#############################################################################

C_SRC  = actions.c cgi.c cgiedit.c cgisimple.c deanimate.c encode.c \
         errlog.c filters.c gateway.c jbsockets.c jcc.c \
         list.c loadcfg.c loaders.c miscutil.c parsers.c ssplit.c \
         urlmatch.c

C_OBJS = $(C_SRC:.c=.o)
C_HDRS = $(C_SRC:.c=.h) project.h actionlist.h

CLIENT_TAG_SRC = client-tags.c
CLIENT_TAG_OBJS = client-tags.o

W32_SRC   = #w32log.c w32taskbar.c win32.c w32svrapi.c
W32_FILES = #w32.res
W32_OBJS  = #$(W32_SRC:.c=.o) $(W32_FILES)
W32_HDRS  = #w32log.h w32taskbar.h win32.h w32res.h w32svrapi.h
W32_LIB   = #-lwsock32 -lcomctl32
W32_INIS  = #config.txt trust.txt

PCRS_SRC     = pcrs.c
PCRS_OBJS    = $(PCRS_SRC:.c=.o)
PCRS_HDRS    = $(PCRS_SRC:.c=.h)

PCRE_SRC     = #pcre/get.c pcre/maketables.c pcre/study.c pcre/pcre.c
PCRE_OBJS    = #$(PCRE_SRC:.c=.o)
PCRE_HDRS    = #pcre/config.h pcre/chartables.c pcre/internal.h pcre/pcre.h

# No REGEX (maybe because dynamically linked pcreposix):
REGEX_SRC    =
#REGEX_SRC = pcre/pcreposix.c

REGEX_OBJS   = $(REGEX_SRC:.c=.o)
REGEX_HDRS   = $(REGEX_SRC:.c=.h)

# Dependencies introduced by #include "project.h".
PROJECT_H_DEPS = project.h $(REGEX_HDRS) $(PCRS_HDRS) #pcre/pcre.h

# Socket libraries for platforms that need them explicitly defined
SOCKET_LIB   = 

# PThreads library, if needed.
PTHREAD_LIB  = 

SRCS         = $(C_SRC) $(CLIENT_TAG_SRC) $(W32_SRC)  $(PCRS_SRC)  $(PCRE_SRC)  $(REGEX_SRC)
OBJS         = $(C_OBJS) $(CLIENT_TAG_OBJS) $(W32_OBJS) $(PCRS_OBJS) $(PCRE_OBJS) $(REGEX_OBJS)
HDRS         = $(C_HDRS) $(W32_HDRS) $(PCRS_HDRS) $(PCRE_OBJS) $(REGEX_HDRS)
LIBS         =  -lz -lpcre -lpcreposix $(W32_LIB) $(SOCKET_LIB) $(PTHREAD_LIB)


#############################################################################
# Compiler switches
#############################################################################

# The flag "-mno-win32" can be used by Cygwin to emulate a un?x type build.
# The flag "-mwindows -mno-cygwin" will cause Cygwin to use MingW32 for a
# Win32 GUI build.
# The flag "-pthread" is required if using Pthreads under Linux (and
# possibly other OSs).
SPECIAL_CFLAGS = -Dunix

# Add your flags here
OTHER_CFLAGS =

CFLAGS = -pipe -O2  $(OTHER_CFLAGS) $(SPECIAL_CFLAGS) -Wall \
         # -Ipcre

LDFLAGS =  $(DEBUG_CFLAGS) $(SPECIAL_CFLAGS)


#############################################################################
# Build section.
#
# There should NOT be any targets above this line.
#############################################################################
all: $(PROGRAM) default.action


#############################################################################
# Phony targets
#############################################################################
.PHONY: all inifiles \
win-dist tarball-dist dok webserver clean clobber tags \
install CONF_DEST LOG_DEST \
PID_DEST check_doc install-strip uninstall GROUP_T

#############################################################################
# Define this explicitly because Solaris is broken!
#############################################################################
%.o: %.c
	$(CC) -c $(CFLAGS) $(CPPFLAGS) $< -o $@


#############################################################################
# Strip master copy comments from default.action:
#############################################################################
default.action: default.action.master
	$(GREP) -v '^#MASTER#' default.action.master > $@
#############################################################################
# Win32 config files
#############################################################################

inifiles: $(W32_INIS)

config.txt: config
	$(SED) -e 's!\trustfile trust!trustfile trust.txt!' \
	       -e 's!\logfile logfile!logfile privoxy.log!' \
	       -e 's!#Win32-only: !!' \
	       < $< | \
	       $(DOSFILTER) > $@
	# LF to CRLF in default.action
	$(DOSFILTER) <default.action >default.action.txt && mv default.action.txt default.action
	# LF to CRLF in default.filter
	$(DOSFILTER) <default.filter >default.filter.txt && mv default.filter.txt default.filter

trust.txt: trust
	$(DOSFILTER) < $< > $@

#############################################################################
# Pre-dist check:
#############################################################################
dist-check:
	@if [ -d CVS ]; then \
           $(ECHO) "***************************************************"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "***                  WARNING                    ***"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "*** The presence of a CVS subdirectory suggests ***"; \
           $(ECHO) "*** that you are trying to build a distribution ***"; \
           $(ECHO) "*** package based on a checked out, not an      ***"; \
           $(ECHO) "*** exported copy of the source tree. Please    ***"; \
           $(ECHO) "*** see \"Releasing a new version\" in the        ***"; \
           $(ECHO) "*** developer manual.                           ***"; \
           $(ECHO) "***                                             ***"; \
           $(ECHO) "***************************************************"; \
           $(ECHO) "Type \"yes i am sure\" if you are sure that you"; \
           $(ECHO) -n "really want to proceed: "; \
           read answer; \
           if [ "$$answer" != "yes i am sure" ]; then exit 1; fi \
         fi;


#############################################################################
# create tar.gz from CVS:
# This make-target is usually called through 'create-archive'. If you
# run 'make create-snapshot' without setting SNAPVERSION, you'll get a
# tar.gz with the current date in the name.
# The main usage is to run it as follows (Red Hat example):
# make SNAPVERSION=1.6x create-snapshot
# This creates a tar.gz.
#############################################################################
create-snapshot:
	@tag=`cvs -d $(CVSROOT) status Makefile | awk ' /Sticky Tag/ { print $$3 } '` 2> /dev/null; \
	[ x"$$tag" = x"(none)" ] && tag=HEAD; \
	echo "*** Creating package from $$tag!"; \
	TMPDIR=$(shell mktemp -d /tmp/$(PROGRAM).XXXXXX); \
	cd $$TMPDIR ; cvs -Q -d $(CVSROOT) export -r $$tag current || echo "Um... export aborted."; \
	cd $$TMPDIR/current; \
	$(TAR) --exclude ".cvsignore" --exclude "CVS" \
		-czf /tmp/$(PROGRAM)-$(VERSION).tar.gz .; \
	$(RM) -rf $$TMPDIR
	@echo "Resulting file is /tmp/$(PROGRAM)-$(VERSION).tar.gz"


#############################################################################
# looks at the version of Makefile and exports a corresponding source-tree
# example: if the Makefile has the sticky tag v_2_9_13, you'll get
# privoxy-*-2.4.13.tar.gz.
#############################################################################
create-archive:
	make SNAPVERSION=$(SNAPVERSION) create-snapshot

#############################################################################
# generic distribution
#############################################################################
gen-dist: dist-check
	@$(ECHO) ""
	@$(ECHO) "You have run autoconf && autoheader && ./configure right?"
	@$(ECHO) ""
	$(MAKE) $(PROGRAM)
	$(STRIP_PROG) $(PROGRAM)
	$(LN) -s `pwd` ../privoxy-$(VERSION)-$(CODE_STATUS)
# add program
	(cd .. && $(TAR) --exclude "PACKAGERS" -cvhf $(GEN_DIST_TAR_NAME) privoxy-$(VERSION)-$(CODE_STATUS)/$(PROGRAM))
# add config files
	for foo in $(CONFIG_FILES); do \
		(cd .. && $(TAR) --exclude "PACKAGERS" -uvhf $(GEN_DIST_TAR_NAME) privoxy-$(VERSION)-$(CODE_STATUS)/$$foo;) \
	done;
# add documentation
	for foo in $(DOC_FILES); do \
		(cd .. && $(TAR) --exclude "PACKAGERS" -uvhf $(GEN_DIST_TAR_NAME) privoxy-$(VERSION)-$(CODE_STATUS)/$$foo;) \
	done;
# add tools
	(cd .. && $(TAR) -uvhf $(GEN_DIST_TAR_NAME) privoxy-$(VERSION)-$(CODE_STATUS)/tools)
# and zip the archive
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS)
	$(GZIP_PROG) ../$(GEN_DIST_TAR_NAME)
	@$(ECHO) Distribution with binary created.

# anonymously ncftps the package to sourceforge
gen-upload:
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming ../$(GEN_DIST_TAR_NAME).gz
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) https://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------

# use with care
gen-clean:
	$(RM) ../$(GEN_DIST_TAR_NAME)*

#############################################################################
# Tarball distribution: No CVS dirs, dotfiles, debian build dir,
# (FIXME:) only parts of the static / generated docs mix in doc/webserver
#############################################################################

tarball-dist: dist-check clean clobber
	$(LN) -s `pwd` ../privoxy-$(VERSION)-$(CODE_STATUS)

	for i in `find . -type f -a -not \( -path "*/CVS*" -o -name ".*" \
	-o -path "*/debian/*" -o -path "*/actions/*" -o -name "*.php" -o \
	-name "PACKAGERS" -o -path "*.git/*" \) | sort`; do \
	   files="$$files privoxy-$(VERSION)-$(CODE_STATUS)/$$i"; \
	done &&  \
	cd .. && $(TAR) -cvhf privoxy-$(VERSION)-$(CODE_STATUS)-src.tar $$files ; \

# and zip the archive
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS)
	$(GZIP_PROG) ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar
	@$(ECHO) Tarball distribution created.

# anonymously ncftps the tarball to sourceforge
tarball-upload:
	ncftpput -u anonymous -p ijbswa-developers@lists.sourceforge.net upload.sourceforge.net /incoming ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar.gz
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) Now goto
	@$(ECHO) https://sourceforge.net/project/admin/editpackages.php?group_id=11118
	@$(ECHO) ... and release the files.
	@$(ECHO) -------------------------------------------------------

tarball-clean:
	$(RM) ../privoxy-$(VERSION)-$(CODE_STATUS)-src.tar.gz

#############################################################################
#
# Documentation
#
# converts doc/source/*.sgml into html and man pages
#
#############################################################################

# developer manual
dok-devel:
	$(RM) doc/webserver/developer-manual/*.html
	$(RM) -r doc/source/developer-manual
	mkdir -p doc/source/developer-manual
	cd doc/source/developer-manual && $(DB) ../developer-manual.sgml && cd .. && cp developer-manual/*.html ../webserver/developer-manual/

# user manual
dok-user:
	$(RM) doc/webserver/user-manual/*.html
	$(RM) -r doc/source/user-manual/
	mkdir -p doc/source/user-manual
	cd doc/source/user-manual && $(DB) -iuser-man ../user-manual.sgml && cd .. && cp user-manual/*.html ../webserver/user-manual/
	# FIXME: temp fix so same stylesheet gets in more than one place so it works
	# for all doc set-ups, including the 'user manual' config option in local
	# system where it MUST be in same directory as html.
	$(PERL) -pi.bak -e 's/<\/head/\n<LINK REL=\"STYLESHEET\" TYPE=\"text\/css\" HREF=\"p_doc.css\">\n<\/head/i' doc/webserver/user-manual/*html && \
	rm doc/webserver/user-manual/*html.bak

# faq
dok-faq:
	$(RM) doc/webserver/faq/*.html
	$(RM) -r doc/source/faq
	mkdir -p doc/source/faq
	cd doc/source/faq && $(DB) ../faq.sgml && cd .. && cp faq/*.html ../webserver/faq/

# man page, one variation. Try to use the next target, just 'make man'.
dok-man:
	$(RM) doc/man/* doc/webserver/man-page/*.html
	echo MAN2HTML is $(MAN2HTML)
	@if [ $(MAN2HTML) != "false" ]; then \
		$(ECHO) "<html><head><title>Privoxy Man page</title><link rel=\"stylesheet\" type=\"text/css\" href=\"../p_web.css\"></head><body><H2>NAME</H2>" > doc/webserver/man-page/privoxy-man-page.html; \
		man ./$(MAN_PAGE) | $(MAN2HTML) -bare >> doc/webserver/man-page/privoxy-man-page.html; \
		$(ECHO) "</body></html>" >> doc/webserver/man-page/privoxy-man-page.html; \
	else \
		$(MAKE) groff2html; \
	fi;

# Build man page from sgml. This requires the SGMLSpm perl module.
# See CPAN, or your favorite perl repository. This is the preferred
# target for man page generation!
man: dok-release
	mkdir -p doc/source/temp && cd doc/source/temp && $(RM) * ;\
	nsgmls ../privoxy-man-page.sgml  | sgmlspl ../../../utils/docbook2man/docbook2man-spec.pl &&\
	perl -pi.bak -e 's/ <URL:.*>//; s/\[ /\[/g' $(MAN_PAGE) ;\
	perl -pi.bak -e "s/\[ /\[/g;s/�/\\\\['a]/g;s/�/\\\\['e]/g" $(MAN_PAGE); \
	perl -pi.bak -e "s/�/\\\\[:o]/g" $(MAN_PAGE); \
	perl -pi.bak -e 's/([ {])-([a-z])/$$1\\-$$2/g' $(MAN_PAGE); \
	perl -pi.bak -e 's/ --([a-z])/ \\-\\-$$1/g' $(MAN_PAGE); \
	perl -pi.bak -e 's/\\fB--/\\fB\\-\\-/g' $(MAN_PAGE); \
	$(DB) ../privoxy-man-page.sgml && $(MV) -f $(MAN_PAGE) ../../../$(MAN_PAGE)

# For those with man2html ala RH7s.
man2html:
	mkdir -p doc/webserver/man-page
	@if [ $(MAN2HTML) != "false" ]; then \
		$(MAN2HTML) $(MAN_PAGE) |grep -v "^Content-type" > tmp.html; \
		$(PERL) -pi.bak -e 's/<A .*Contents<\/A>//; s/<A .*man2html<\/A>/man2html/' tmp.html; \
		$(PERL) -pi.bak -e 's/(<\/HEAD>)/<LINK REL=\"STYLESHEET\" TYPE=\"text\/css\" HREF=\"..\/p_doc.css\"><\/HEAD>/' tmp.html; \
		$(PERL) -pi.bak -e 's/(<A.*),(">)/$$1$$2/g' tmp.html; \
		$(PERL) -pi.bak -e 's,\.">,">,g' tmp.html; \
		$(PERL) -pi.bak -e "s/\['a\]/\&aacute;/g;s/\['e\]/\&eacute;/g" tmp.html; \
		$(SED) -e 's///g' tmp.html > doc/webserver/man-page/privoxy-man-page.html && $(RM) tmp.*; \
	else \
		$(MAKE) groff2html; \
	fi;

# Otherwise we get plain groff conversion.
groff2html:
	$(G2H_CMD) ./$(MAN_PAGE) | $(SED) -e 's@</head>@<link REL="STYLESHEET" TYPE="text/css" HREF="../p_doc.css"></head>@' > doc/webserver/man-page/privoxy-man-page.html


# readme page and INSTALL file
dok-readme: dok-release
	cd doc/source && $(DB)-notoc -V nochunks readme.sgml > tmp.html &&\
	$(W3M_DUMP) tmp.html > ../../README ;\
	$(DB)-notoc -V nochunks install.sgml > tmp.html &&\
	$(W3M_DUMP) tmp.html > ../../INSTALL ;\
	$(RM) tmp.*

# index.sgml is used to create both the Home Page, and a local index
# for documentation, etc.
#
# index.html for webserver:
dok-webserver:
	cd doc/source/webserver && $(DB)-notoc -ip-homepage -V nochunks index.sgml > ../../webserver/index.html
	$(PERL) -pi.bak -e 's/..\/p_doc.css/p_doc.css/;\
	s/<\/HEAD/\n<meta name=\"description\" content=\"Privoxy helps users to protect their privacy.\"><\/HEAD/;\
	s/\.\d\. //;\
	s/__copy/&copy;/;\
	s@(<SUB)@<p style="text-align: center">\1@; s@(</SUB)@\1></p@;\
        s@ChameleonJohn@<br><a href="http://www.chameleonjohn.com/"><img align="middle"\
 src="images/sponsors/chameleonjohn.png" alt="ChameleonJohn Coupons"></a>@' \
     doc/webserver/index.html && $(RM) doc/webserver/*.bak

# privoxy-index.html for local documentation:
dok-index:
	cd doc/source/webserver && $(DB)-notoc -ip-index -V nochunks index.sgml > ../../webserver/privoxy-index.html
	$(PERL) -pi.bak -e 's/..\/p_doc.css/p_doc.css/;\
	s/<\/HEAD/\n<meta name=\"description\" content=\"Privoxy helps users to protect their privacy.\"><\/HEAD/;\
	s/\.\d\. //;\
	s/__copy/&copy;/' \
     doc/webserver/privoxy-index.html && $(RM) doc/webserver/*.bak

# Main documentation target.
dok: dok-release dok-devel dok-user dok-faq dok-readme dok-webserver dok-authors dok-index
	@$(ECHO) Documentation created.

## Make AUTHORS file
dok-authors:
	cd doc/source && $(DB) -V nochunks authors.sgml > tmp.html && $(W3M_DUMP_UTF8) \
	  tmp.html > ../../AUTHORS && $(RM) tmp.html

# Set doc entities for VERSION and CODE_STATUS in sgml docs. Toggle content
# exceptions accordingly. This needs to go before any doc building (doh).
dok-release:
	@$(ECHO) Setting doc version and status to $(VERSION), $(CODE_STATUS)
	@$(PERL) -pi.bak -e 's/<!entity +p-version.*>/<!entity p-version "$(VERSION)">/;\
     s/<!entity +p-status.*>/<!entity p-status "$(CODE_STATUS)">/' \
     doc/source/*sgml doc/source/*/*sgml
	$(RM) -r doc/source/*bak doc/source/*/*bak
	@if [ $(CODE_STATUS) = "stable" ]; then \
		$(ECHO) Setting docs to stable $(VERSION); \
		$(PERL) -pi.bak -e 's/<!entity +% +p-stable.*>/<!entity % p-stable "INCLUDE">/;\
			s/<!entity +% +p-not-stable.*>/<!entity % p-not-stable "IGNORE">/' \
		doc/source/*sgml doc/source/*/*sgml; \
	else \
		$(ECHO) Setting docs to not stable $(VERSION); \
		$(PERL) -pi.bak -e 's/<!entity +% +p-stable.*>/<!entity % p-stable "IGNORE">/; \
			s/<!entity +% +p-not-stable.*>/<!entity % p-not-stable "INCLUDE">/' \
		doc/source/*sgml doc/source/*/*sgml; \
	fi;
	$(RM) -r doc/source/*bak doc/source/*/*bak;

# The main Privoxy config file, generated from sgml sources.
# NOTE: This will require some hand editing.
config-file: dok-release generate-config-file

	$(RM) config.bak config.html
	@$(ECHO)  "****************************************************"
	@$(ECHO)  "The config file has been optimistically updated"
	@$(ECHO)  "Now -- you may need to hand edit the results!"
	@$(ECHO)  "In particular, check the Debug levels, the"
	@$(ECHO)  "permit-access, forward & socks examples and the"
	@$(ECHO)  "various user-manual examples, which all"
	@$(ECHO)  "might have gotten hammered."
	@$(ECHO)  "****************************************************"

generate-config-file:
	cd doc/source && $(DB)-notoc -iconfig-file -V nochunks config.sgml > ../../config.html
	$(W3M_DUMP) -cols 67 config.html > config
	$(PERL) -i.bak utils/prepare-configfile.pl config

#############################################################################
#
# Webserver
#
# moves documentation to webserver
#
#############################################################################
sf-shell:
	@sf_name=$(SOURCE_FORGE_NAME); \
	[ -n "$${sf_name}" ] || read -p "Enter SourceForge username: " sf_name || exit 1; \
	echo "Opening shell for $${sf_name} ..."; \
	ssh -t $${sf_name},ijbswa@shell.sourceforge.net create

webserver: clean-editor-files
	@$(ECHO) -------------------------------------------------------
	@$(ECHO) You will need to "create" a SF shell first:
	@$(ECHO)    ssh -t SF-USER-ID,ijbswa@shell.sourceforge.net create
	@$(ECHO) Please make sure your documentation files are up to date.
	@$(ECHO) Note that this command updates the home page and copys
	@$(ECHO) all stuff to the webserver, it will not remove obsolete documents.
	@$(ECHO) Note that a botched upload can result in the documentation
	@$(ECHO) on the website becoming unreachable! Also the CSS files
	@$(ECHO) currently seem to end up at the wrong place.
	@$(ECHO) -------------------------------------------------------

	@$(ECHO) Replacing the user-manual symlink
	@$(SSH) shell.sourceforge.net "cd $(WWW_ROOT)/htdocs && rm user-manual \
	 && mkdir -p $(VERSION)/user-manual && ln -s $(VERSION)/user-manual user-manual"

	@$(ECHO) Uploading html
	@cd doc/webserver; \
          upload=`find . -type f -a -not \( -path "*/CVS*" -o -path "*/results*" \)`; \
          $(TAR) cf - $$upload | $(SSH) shell.sourceforge.net 'cd $(WWW_ROOT)/htdocs/; tar xvm 2>&1 | grep -v timestamp'

	@$(ECHO) Fixing permissions
	@$(SSH) shell.sourceforge.net 'chmod -R 775 $(WWW_ROOT)/htdocs 2>/dev/null; true'
	@$(SSH) shell.sourceforge.net 'find $(WWW_ROOT)/htdocs/ -type f | xargs chmod 664 2>/dev/null; true'

web-actions:
	@$(ECHO) Updating the actions on the webserver ...
	@$(RSYNC) doc/webserver/actions/*.php shell.sourceforge.net:$(WWW_ROOT)/htdocs/actions
	@$(ECHO) Enforcing reasonable permissions ...
	@$(SSH) shell.sourceforge.net 'find $(WWW_ROOT)/htdocs/actions/ -type f | xargs chmod 664 2>/dev/null'

web-faq:
	@$(ECHO) Updating the FAQ on the webserver ...
	@$(RSYNC) doc/webserver/faq/*.html shell.sourceforge.net:$(WWW_ROOT)/htdocs/faq
	@$(ECHO) Enforcing reasonable permissions ...
	@$(SSH) shell.sourceforge.net 'find $(WWW_ROOT)/htdocs/faq/ -type f | xargs chmod 664 2>/dev/null'

web-user-manual:
	@$(ECHO) Updating the user manual on the webserver (do not use in case of version changes) ...
	@$(RSYNC) doc/webserver/user-manual/*.html shell.sourceforge.net:$(WWW_ROOT)/htdocs/user-manual/
	@$(ECHO) Enforcing reasonable permissions ...
	@$(SSH) shell.sourceforge.net 'find $(WWW_ROOT)/htdocs/user-manual/ -type f | xargs chmod 664 2>/dev/null'

#############################################################################
#
# Try to clean up the generated HTML files.
#
# The files are such a mess that some of them require two tidy runs in a
# row as the first one aborts prematurely. The vanilla tidy output renders
# poorly because it contains a bit too much whitespace, so we additionally
# run the files through perl to fix this again.
#
#############################################################################
dok-tidy:
	for html_file in `find doc/webserver -name "*.html"`; do \
		$(TIDY) $$html_file || $(TIDY) $$html_file; \
		$(PERL) -i'' -e 's@^\s*<br>\s*$$@@; s@ +$$@@;' -n -p $$html_file; \
	done


#############################################################################
# Source file dependencies
#############################################################################

actions.o:   actions.c   actions.h   config.h $(PROJECT_H_DEPS) errlog.h filters.h jcc.h list.h loaders.h miscutil.h actionlist.h ssplit.h
cgi.o:       cgi.c       cgi.h       config.h $(PROJECT_H_DEPS) cgiedit.h cgisimple.h jbsockets.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actions.h errlog.h miscutil.h
cgiedit.o:   cgiedit.c   cgiedit.h   config.h $(PROJECT_H_DEPS) cgi.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actionlist.h actions.h errlog.h miscutil.h
cgisimple.o: cgisimple.c cgisimple.h config.h $(PROJECT_H_DEPS) cgi.h list.h pcrs.h encode.h ssplit.h jcc.h filters.h actions.h errlog.h miscutil.h urlmatch.h
deanimate.o: deanimate.c deanimate.h config.h $(PROJECT_H_DEPS)
encode.o:    encode.c    encode.h    config.h
errlog.o:    errlog.c    errlog.h    config.h $(PROJECT_H_DEPS) #w32log.h
filters.o:   filters.c   filters.h   config.h $(PROJECT_H_DEPS) errlog.h encode.h gateway.h jbsockets.h jcc.h loadcfg.h parsers.h ssplit.h cgi.h deanimate.h urlmatch.h #win32.h
gateway.o:   gateway.c   gateway.h   config.h $(PROJECT_H_DEPS) errlog.h jbsockets.h jcc.h loadcfg.h
jbsockets.o: jbsockets.c jbsockets.h config.h $(PROJECT_H_DEPS) filters.h
jcc.o:       jcc.c       jcc.h       config.h $(PROJECT_H_DEPS) errlog.h filters.h gateway.h jbsockets.h loadcfg.h loaders.h miscutil.h parsers.h #w32log.h win32.h w32svrapi.h cgi.h
list.o:      list.c      list.h      config.h $(PROJECT_H_DEPS) list.h miscutil.h
loadcfg.o:   loadcfg.c   loadcfg.h   config.h $(PROJECT_H_DEPS) errlog.h filters.h gateway.h jbsockets.h jcc.h loaders.h miscutil.h parsers.h #w32log.h win32.h
loaders.o:   loaders.c   loaders.h   config.h $(PROJECT_H_DEPS) errlog.h encode.h filters.h gateway.h jcc.h loadcfg.h miscutil.h parsers.h ssplit.h
miscutil.o:  miscutil.c  miscutil.h  config.h
parsers.o:   parsers.c   parsers.h   config.h $(PROJECT_H_DEPS) errlog.h filters.h jbsockets.h jcc.h loadcfg.h loaders.h miscutil.h ssplit.h
ssplit.o:    ssplit.c    ssplit.h    config.h miscutil.h
urlmatch.o:  urlmatch.c  urlmatch.h  config.h $(PROJECT_H_DEPS) errlog.h miscutil.h ssplit.h

# GNU regex
gnu_regex.o: gnu_regex.c gnu_regex.h config.h

# PCRS
pcrs.o: pcrs.c pcrs.h config.h #pcre/pcre.h

# PCRE
pcre/get.o:        pcre/get.c        pcre/config.h pcre/internal.h pcre/pcre.h
pcre/maketables.o: pcre/maketables.c pcre/config.h pcre/internal.h pcre/pcre.h
pcre/pcre.o:       pcre/pcre.c       pcre/config.h pcre/internal.h pcre/pcre.h pcre/chartables.c
pcre/pcreposix.o:  pcre/pcreposix.c  pcre/config.h pcre/internal.h pcre/pcre.h pcre/pcreposix.h
pcre/study.o:      pcre/study.c      pcre/config.h pcre/internal.h pcre/pcre.h

# An auxiliary program makes the PCRE default character table source

pcre/chartables.c:   pcre/dftables
		pcre/dftables >pcre/chartables.c

pcre/dftables:       pcre/dftables.c pcre/maketables.c pcre/pcre.h pcre/internal.h pcre/config.h
		$(CC) -o pcre/dftables $(CFLAGS) pcre/dftables.c

# Win32
w32log.o: w32log.c errlog.h config.h jcc.h loadcfg.h miscutil.h pcre/pcre.h pcre/pcreposix.h pcrs.h project.h w32log.h w32taskbar.h win32.h
w32taskbar.o: w32taskbar.c config.h w32log.h w32taskbar.h
win32.o: win32.c config.h jcc.h loadcfg.h pcre/pcre.h pcre/pcreposix.h pcrs.h project.h w32log.h win32.h w32svrapi.h

w32.res: w32.rc w32res.h icons/radar-01.ico icons/radar-02.ico icons/radar-03.ico icons/radar-04.ico icons/radar-05.ico icons/radar-06.ico icons/radar-07.ico icons/radar-08.ico icons/idle.ico icons/privoxy.ico config.h
	windres -D__MINGW32__=0.2 -O coff -i $< -o $@

# AmigaOS
#OBJS += amiga.o
#ifeq ($(shell $(CC) -dumpmachine), m68k-amigaos)
#CFLAGS += -D__AMIGAVERSION__=\"$(VERSION_MAJOR).$(VERSION_MINOR)$(VERSION_POINT)\" -D__AMIGADATE__=\"`date +%d.%m.%Y`\" -W -m68020 -noixemul -fbaserel -msmall-code
#LDFLAGS += -m68020 -noixemul -fbaserel
#LIBS = -lm /gg/lib/libb/libm020/libnix/swapstack.o
#else
#CFLAGS += -D__AMIGAVERSION__=\"$(VERSION_MAJOR).$(VERSION_MINOR)$(VERSION_POINT)\" -D__AMIGADATE__=\"`date +%d.%m.%Y`\" -Wextra -D__USE_INLINE__ -D__NO_INTUITION_RJ_MACROS
#endif
#amiga.o: amiga.c amiga.h config.h


$(PROGRAM): $(OBJS) $(W32_FILES)
	$(LD) $(LDFLAGS) -o $(PROGRAM) $(OBJS) $(LIBS)

clean:
	$(RM) a.out $(OBJS) $(W32_FILES) $(W32_INIS) $(PROGRAM) default.action \
		config.base config.tmp \
		`find . \( -name TAGS -o -name tags \) -a -not -path "./.git/*"` \
		`find . -name "*.orig" -a -not -name rc.privoxy.orig -a -not -path "./.git/*"`

clean-editor-files:
	$(RM) `find . -name "*~"`
	$(RM) `find . -name "#*#"` # Emacs backup files
	$(RM) `find . -name ".\#*"`

clobber: clean-editor-files
	$(RM) GNUmakefile configure config.h.in config.h config.cache config.status config.log logfile \
              privoxy.log core *.tar.gz *.tar privoxy-cl.spec doc/source/ldp.dsl config.new
	$(RM) -r autom4te.cache

#
# FIXME: What is all this?
#
	$(RM) cscope.*  *.pdb *.lib *.exp

distclean: clobber

tags: $(SRCS) $(HDRS)
	etags $(SRCS) $(HDRS)

CONF_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(CONF_BASE)" = "$(prefix)/etc" ];then \
		$(ECHO) "$(CONF_BASE)/privoxy";\
	 else\
		 $(ECHO) "$(CONF_BASE)";\
	 fi)

LOG_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(LOGS_DEST)" = "$(prefix)/var/log/privoxy" ];then \
		$(ECHO) "/var/log/privoxy" ;\
	 else\
		 $(ECHO) "$(LOGS_DEST)";\
	 fi)

PID_DEST:=$(shell if [ "$(prefix)" = "/usr/local" ] && [ "$(PIDS_DEST)" = "$(prefix)/var/run" ];then \
		$(ECHO) "/var/run" ;\
	 else\
		 $(ECHO) "$(PIDS_DEST)";\
	 fi)

check_doc:=$(shell if [ ! -d "$(SHARE_DEST)/doc" ] && [ "$(prefix)" = "/usr/local" ]  && [ -d "$(prefix)/doc" ];then \
		$(ECHO) "1";\
	 else\
		 $(ECHO) "0";\
	 fi)

# If USER is specified but no GROUP, assume there is a GROUP of same name.
GROUP_T:=$(shell if [ x$(GROUP) = x ] && [ x$(USER) != x ];then \
		$(ECHO) "$(USER)" ;\
	 else\
		 $(ECHO) "$(GROUP)";\
	 fi)

install-strip:
	$(MAKE) install STRIP=-s

# FIXME: Test USER and GROUP on Slack to make sure this works as
# intended.
#
# FIXME: id handling needs help, probably via configure, since 'id -u' is not
# universally reliable (eg Solaris). Group handling could be better.
# Perhaps the whole user/group validation should be done here, and simplified.
PROGRAM_V = Privoxy $(VERSION) $(CODE_STATUS)
install: CONF_DEST LOG_DEST PID_DEST check_doc GROUP_T
	@# Quick test for valid USER.
	@if [ -n "$(USER)" ]; then \
		$(ID) $(USER) >/dev/null || exit 1;\
	fi
	@# Test for valid group. FIXME. USER does not have to belong to GROUP
	@# for file ownership purposes.
# 	if [ -n "$(GROUP_T)" ] && [ -n "$(USER)" ] && ! $(GROUPS) $(USER) | $(GREP) "\<$(GROUP_T)\>" >/dev/null; then \
# 		$(ECHO) Group $(GROUP_T) for User $(USER) is invalid && exit 1 ;\
# 	fi

	@$(ECHO) "Creating directories, and preparing $(PROGRAM_V) installation"
	$(CHMOD) $(DIR_MODE) $(MKDIR)
	@$(MKDIR) $(DESTDIR)$(SBIN_DEST) $(DESTDIR)$(prefix) $(DESTDIR)$(CONF_DEST) \
		$(DESTDIR)$(CONF_DEST)/templates $(DESTDIR)$(SHARE_DEST) \
		$(DESTDIR)$(LOG_DEST) $(DESTDIR)$(PID_DEST)
	@# Install the executable binary, strip if invoked as install-strip
	@test -n "$(STRIP)" &&\
	$(ECHO) Installing $(PROGRAM) stripped executable to $(SBIN_DEST) ||\
	$(ECHO) Installing $(PROGRAM) executable to $(DESTDIR)$(SBIN_DEST)
	$(INSTALL) $(INSTALL_P) $(STRIP) $(PROGRAM) $(DESTDIR)$(SBIN_DEST)

	@# Install the DOCS and man page. install-sh only does one file at a time.
	@# FIXME: only handles jpegs.
	-@if [ $(check_doc) = 0 ]; then \
		DOC=$(DOC_DEST) ;\
	else \
		DOC=$(prefix)/doc/privoxy ;\
	fi;\
	$(MKDIR) $(DESTDIR)$$DOC $(DESTDIR)$$DOC/user-manual $(DESTDIR)$$DOC/faq $(DESTDIR)$$DOC/developer-manual \
	     $(DESTDIR)$$DOC/man-page $(DESTDIR)$$DOC/images $(DESTDIR)$(MAN_DEST) ;\
	if [ -d "$(DOK_WEB)" ]; then \
		$(ECHO) Installing FAQ, Manual, and other docs to $(DESTDIR)$$DOC;\
          for i in user-manual developer-manual faq; do \
               for ii in $(DOK_WEB)/$$i/*html; do \
                    $(INSTALL) $(INSTALL_T) $$ii $(DESTDIR)$$DOC/$$i;\
               done ;\
          done ;\
		for i in $(DOK_WEB)/user-manual/*jpg; do \
               $(INSTALL) $(INSTALL_T) $$i $(DESTDIR)$$DOC/user-manual;\
          done ;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/man-page/*html $(DESTDIR)$$DOC/man-page;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/privoxy-index.html $(DESTDIR)$$DOC/index.html;\
		$(INSTALL) $(INSTALL_T) AUTHORS $(DESTDIR)$$DOC;\
		$(INSTALL) $(INSTALL_T) LICENSE $(DESTDIR)$$DOC;\
		$(INSTALL) $(INSTALL_T) README $(DESTDIR)$$DOC;\
		$(INSTALL) $(INSTALL_T) ChangeLog $(DESTDIR)$$DOC;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/p_doc.css $(DESTDIR)$$DOC;\
		$(INSTALL) $(INSTALL_T) $(DOK_WEB)/p_doc.css $(DESTDIR)$$DOC/user-manual;\
	fi
	@# Not all platforms support gzipped man pages.
	@$(ECHO) Installing man page to $(DESTDIR)$(MAN_DEST)/$(MAN_PAGE)
	-$(INSTALL) $(INSTALL_T) $(MAN_PAGE)  $(DESTDIR)$(MAN_DEST)/$(MAN_PAGE)

	@# Change the config file default directories according to the configured ones
	@$(ECHO) Rewriting config for this installation
	@if [ -f config.base ] ; then \
		$(CAT) config >config~ ;\
		$(MV) config.base config ;\
	fi
	$(SED) 's+^confdir \.+confdir $(CONF_DEST)+' config | \
	$(SED) 's+^logdir \.+logdir $(LOG_DEST)+' >config.tmp
	-@if [ $(check_doc) = 0 ]; then \
      $(SED) 's+^#\?user-manual .*+user-manual $(DOC_DEST)/user-manual/+' config.tmp >config.updated ;\
	else \
      $(SED) 's+^#\?user-manual .*+user-manual $(prefix)/doc/privoxy/user-manual/+' config.tmp >config.updated ;\
	fi;\
	$(MV) config config.base
	$(MV) config.updated config

	@# Install the config support files. Test for root install, and abort
	@# if there is no privoxy user, and no other user was enabled during
	@# configure. Later, install init script if appropriate.
	@$(ECHO) Installing templates to $(DESTDIR)$(CONF_DEST)/templates
	@for i in `find templates -type f`; do \
		$(INSTALL) $(INSTALL_T) $$i $(DESTDIR)$(CONF_DEST)/templates ;\
	done

	@# FIXME: group/user validation is overly convoluted.
	@# If superuser install ... we require a minimum of group ownership
	@# of those files the daemon writes to, to be non-root owned.
	@if [ "`$(ID) |sed 's/(.*//' |sed 's/.*=//'`" = "0" ] ;then\
		if [ x$(USER) = x ] || [ $(USER) = root ]; then \
			if [ x$(GROUP) = x ] || [ $(GROUP) = root ]; then \
				if [ "`$(ID) privoxy`" ] && \
					$(GROUPS) privoxy | $(SED) 's/^.*://' |$(GREP) "\<privoxy\>" >/dev/null; then \
					$(ECHO) "Warning: Setting group owner to privoxy";\
					GROUP_T=privoxy ;\
				else \
					$(ECHO)  "******************************************************************" ;\
					$(ECHO)  " WARNING! WARNING! installing config files as root!" ;\
					$(ECHO)  " It is strongly recommended to run $(PROGRAM) as a non-root user," ;\
					$(ECHO)  " and to install the config files as that user and/or group!" ;\
					$(ECHO)  " Please read INSTALL, and create a privoxy user and group!" ;\
					$(ECHO)  "*******************************************************************" ;\
					exit 1 ;\
				fi ;\
			else \
				GROUP_T=$(GROUP) ;\
			fi ;\
			INSTALL_CONF="$(INSTALL_R) -g $$GROUP_T " ;\
		else \
			$(ECHO) "Superuser install, installing config files as $(USER):$(GROUP_T)" ;\
			INSTALL_CONF="$(INSTALL_R) -o $(USER) -g $(GROUP_T)" ;\
			GROUP_T=$(GROUP_T) ;\
		fi ;\
	else \
		if [ ! "`id $(USER)`" = "`id`" ] ;then \
			$(ECHO) "** WARNING ** current install user different from configured user!!" ;\
			$(ECHO) "Edit may fail." ;\
		fi ;\
		INSTALL_CONF="$(INSTALL_R)" ;\
	fi ;\
	$(ECHO) Installing configuration files to $(DESTDIR)$(CONF_DEST);\
	for i in $(CONFIGS); do \
		if [ "$$i" = "default.action" ] || [ "$$i" = "default.filter" ] ; then \
			$(RM) $(DESTDIR)$(CONF_DEST)/$$i ;\
			$(ECHO) Installing fresh $$i;\
			$(INSTALL) $$INSTALL_CONF $$i $(DESTDIR)$(CONF_DEST) || exit 1;\
		elif [ -s "$(CONF_DEST)/$$i" ]; then \
			$(ECHO) Installing $$i as $$i.new ;\
			$(INSTALL) $$INSTALL_CONF $$i $(DESTDIR)$(CONF_DEST)/$$i.new || exit 1;\
			NEW=1;\
		else \
			$(INSTALL) $$INSTALL_CONF $$i $(DESTDIR)$(CONF_DEST) || exit 1;\
		fi ;\
	done ;\
	if [ -n "$$NEW" ]; then \
		$(CHMOD) $(RWD_MODE) $(DESTDIR)$(CONF_DEST)/*.new || exit 1 ;\
		$(ECHO) "Warning: Older config files are preserved. Check new versions for changes!" ;\
	fi ;\
	[ ! -f $(DESTDIR)$(LOG_DEST)/logfile ] && $(ECHO) Creating logfiles in $(DESTDIR)$(LOG_DEST) || \
		$(ECHO) Checking logfiles in $(DESTDIR)$(LOG_DEST) ;\
		$(TOUCH) $(DESTDIR)$(LOG_DEST)/logfile || exit 1 ;\
	if [ x$$USER != x ]; then \
		$(CHOWN) $$USER $(DESTDIR)$(LOG_DEST)/logfile || \
		$(ECHO) "** WARNING ** current install user different from configured user. Logging may fail!!" ;\
	fi ;\
	if [ x$$GROUP_T != x ]; then \
		$(CHGRP) $$GROUP_T $(DESTDIR)$(LOG_DEST)/logfile || \
		$(ECHO) "** WARNING ** current install user different from configured user. Logging may fail!!" ;\
	fi ;\
	$(CHMOD) $(RWD_MODE) $(DESTDIR)$(LOG_DEST)/logfile || exit 1 ;\
	if [ "$(prefix)" = "/usr/local" ] || [ "$(prefix)" = "/usr" ]; then \
		if [ -f /etc/slackware-version ] && [ -d /etc/rc.d/ ] && [ -w /etc/rc.d/ ] ; then \
               $(SED) 's+%PROGRAM%+$(PROGRAM)+' slackware/rc.privoxy.orig | \
               $(SED) 's+%SBIN_DEST%+$(SBIN_DEST)+' | \
               $(SED) 's+%CONF_DEST%+$(CONF_DEST)+' | \
               $(SED) 's+%USER%+$(USER)+' | \
               $(SED) 's+%GROUP%+$(GROUP_T)+' >slackware/rc.privoxy ;\
			$(INSTALL) $(INSTALL_P) slackware/rc.privoxy $(DESTDIR)/etc/rc.d/ ;\
               $(ECHO) "Installing for Slackware." ;\
               $(ECHO) "Dont forget to add the rc.privoxy to rc.local if you want it started at every boot" ;\
		elif [ -d $(DESTDIR)/etc/init.d ] && [ -w $(DESTDIR)/etc/init.d ] ; then \
			$(ECHO) "Installing generic init script to $(DESTDIR)/etc/init.d/privoxy" ;\
			$(ECHO) "Please check that the PATHs are correct, and edit if needed." ;\
			$(INSTALL) $(INSTALL_P) privoxy-generic.init $(DESTDIR)/etc/init.d/privoxy ;\
		fi ;\
	else \
		$(ECHO) "No init script installed, install it manually if needed" ;\
	fi
	$(RM) config.base config.tmp
	@# mmmmm, good.
	@$(ECHO) "$(PROGRAM_V) installation succeeded!"
	@$(ECHO) "The Privoxy configuration files have been installed in $(DESTDIR)$(CONF_DEST)"

# rmdir is used as a precaution since it will not remove non-empty
# directories. RH init script creates lock file and pid file.
uninstall: CONF_DEST LOG_DEST PID_DEST check_doc
	@$(ECHO) Starting Privoxy uninstallation
	@# KILL privoxy if running
	@# XXX: the chkconfig line may need a DESTDIR prefix.
	-@test -f $(DESTDIR)$(PID_DEST)/privoxy.pid && $(ECHO) Stopping $(PROGRAM) &&\
         $(KILL) `$(CAT) $(DESTDIR)$(PID_DEST)/privoxy.pid` || :
	-@test -f $(DESTDIR)/var/run/privoxy.pid && $(ECHO) Stopping $(PROGRAM) &&\
          $(KILL) `$(CAT) $(DESTDIR)/var/run/privoxy.pid ` || :

	@# Program binary
	@$(ECHO) Removing $(PROGRAM) binary
	$(RM) $(DESTDIR)$(SBIN_DEST)/$(PROGRAM) $(SBIN_DEST)/$(PROGRAM)~

	@# config files and dir, and maybe old install backups
	-@if [ -d $(DESTDIR)$(CONF_DEST) ]; then \
		$(ECHO) Saving $(PROGRAM) config files to $(DESTDIR)/tmp/$(PROGRAM)-save ;\
		$(MKDIR) $(DESTDIR)/tmp/$(PROGRAM)-save ;\
		cd $(DESTDIR)$(CONF_DEST) ;\
		for i in $(DESTDIR)$(CONFIGS); do \
			[ -f $$i ] && $(CP) $$i $(DESTDIR)/tmp/$(PROGRAM)-save ;\
		done ;\
	fi
	@$(ECHO) Removing $(PROGRAM) config files
	-@for i in $(DESTDIR)$(CONFIGS); do \
		test -f $(CONF_DEST)/$$i && $(ECHO) Removing $$i ;\
		$(RM) $(DESTDIR)$(CONF_DEST)/$$i $(DESTDIR)$(CONF_DEST)/$$i~ $(DESTDIR)$(CONF_DEST)/$$i.new ;\
	done
	-@test -d $(DESTDIR)$(CONF_DEST)/templates && $(RM) -r $(DESTDIR)$(CONF_DEST)/templates &&\
	$(ECHO) "Removing $(DESTDIR)$(CONF_DEST)/templates/*"

	@# man page and docs
	@$(ECHO) Removing $(PROGRAM) docs
	-$(RM) $(DESTDIR)$(MAN_DEST)/$(MAN_PAGE)*
	-$(RM) -r $(DESTDIR)$(DOC_DEST) || $(RM) -r $(DESTDIR)$(prefix)/doc/privoxy

	@# Log and pidfile
	@$(ECHO) Removing $(PROGRAM) logs
	-$(RM) $(DESTDIR)$(LOG_DEST)/logfile $(DESTDIR)$(PID_DEST)/privoxy.pid

	@# Final clean up of unused directories. Special handling of CONF and LOG
     # destinations.
	@$(ECHO) Removing $(PROGRAM) directories
	@for i in $(DESTDIR)$(LOG_DEST) $(DESTDIR)$(CONF_DEST); do \
		if test -d $$i; then \
			$(ECHO) Removing $$i ;\
			$(RMDIR) $$i || $(ECHO) "$$i is not empty, not removed" ;\
		fi;\
	done
	@if [  ! "$(prefix)" = "/usr/local" ] ;then \
		for i in $(DESTDIR)$(MAN_DEST) $(DESTDIR)$(MAN_DIR) $(DESTDIR)$(SHARE_DEST)/doc \
                         $(DESTDIR)$(SHARE_DEST) $(DESTDIR)$(SBIN_DEST); do \
			if test -d $$i; then \
				$(ECHO) Removing $$i ;\
				$(RMDIR) $$i || $(ECHO) "$$i is not empty, not removed" ;\
			fi;\
		done;\
		if test $(LOG_DEST) != /var/log/privoxy && test -d $(DESTDIR)$(prefix)/var/log; then \
			$(ECHO) Removing $(DESTDIR)$(prefix)/var/log ;\
			$(RMDIR) $(DESTDIR)$(prefix)/var/log || $(ECHO) "$(DESTDIR)$(prefix)/var/log is not empty, not removed";\
		fi ;\
		if test $(PID_DEST) != /var/run && test -d $(DESTDIR)$(prefix)/var/run; then \
			$(ECHO) Removing $(DESTDIR)$(prefix)/var/run ;\
			$(RMDIR) $(DESTDIR)$(prefix)/var/run || $(ECHO) "$(DESTDIR)$(prefix)/var/run is not empty, not removed";\
		fi ;\
		if test $(prefix)/var != /var && test -d $(DESTDIR)$(prefix)/var; then \
			$(ECHO) Removing $(DESTDIR)$(prefix)/var ;\
			$(RMDIR) $(DESTDIR)$(prefix)/var || $(ECHO) "$(DESTDIR)$(prefix)/var is not empty, not removed" ;\
		fi ;\
		if test $(prefix) != / && test $(prefix) != /usr && test -d $(DESTDIR)$(prefix); then \
			$(ECHO) Removing $(DESTDIR)$(prefix) ;\
			$(ECHO) Removing installation directory $(DESTDIR)$(prefix) ;\
			$(RMDIR) $(DESTDIR)$(prefix) || $(ECHO) "$(DESTDIR)$(prefix) is not empty, not removed" ;\
		fi;\
	fi

	@# init scripts
	@if [ "$(prefix)" = "/usr/local" ] || [ "$(prefix)" = "/usr" ]; then \
		$(ECHO) Removing $(PROGRAM) init script ;\
		if [ -f $(DESTDIR)/etc/slackware-version ] && \
	                [ -d $(DESTDIR)/etc/rc.d/ ]  && [ -w $(DESTDIR)/etc/rc.d/ ] ; then \
			$(RM) $(DESTDIR)/etc/rc.d/rc.privoxy ;\
		elif [ -d $(DESTDIR)/etc/init.d ] && [ -w $(DESTDIR)/etc/init.d ] ; then \
			$(RM) $(DESTDIR)/etc/init.d/privoxy ;\
		else \
			$(ECHO) "Unable to remove privoxy init script, not installed or permission denied" ;\
		fi ;\
	fi
	@$(ECHO) Privoxy uninstalled, bye

coffee:
	 @perl	-e 'print pack "C*", (31,139,8,8,153,63,226,60,2,3,99,111,102,102,101,'	 \
		-e '101,0,109,143,205,13,192,32,8,133,239,78,241,110,234,1,28,160,171,'  \
		-e '152,208,53,26,117,247,22,165,73,137,125,9,1,62,126,2,128,169,5,243,' \
		-e '143,13,139,49,164,65,100,149,152,102,73,141,88,73,178,116,205,100,'  \
		-e '69,253,36,102,81,49,83,236,19,225,171,131,214,172,163,73,4,168,123,' \
		-e '115,71,126,247,122,94,128,178,227,95,154,12,86,215,122,197,249,146,' \
		-e '187,54,220,125,193,51,228,11,1,0,0);' | zcat
