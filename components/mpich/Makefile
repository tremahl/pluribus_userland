#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL"), version 1.0.
# You may only use this file in accordance with the terms of version
# 1.0 of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source.  A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2013 Aurelien Larcher.  All rights reserved.
#

include ../../make-rules/shared-macros.mk
include $(WS_MAKE_RULES)/mpi-macros.mk

COMPONENT_NAME =	mpich/$(COMPILER)
COMPONENT_VERSION =	3.1.2
COMPONENT_FMRI =	library/$(COMPONENT_NAME)
COMPONENT_PROJECT_URL =	http://www.mpich.org/
COMPONENT_SRC_NAME = mpich
COMPONENT_SRC =		$(COMPONENT_SRC_NAME)-$(COMPONENT_VERSION)
COMPONENT_ARCHIVE =	$(COMPONENT_SRC).tar.gz
COMPONENT_ARCHIVE_HASH = \
    sha256:37c3ba2d3cd3f4ea239497d9d34bd57a663a34e2ea25099c2cbef118c9156587
COMPONENT_ARCHIVE_URL =	http://www.mpich.org/static/downloads/$(COMPONENT_VERSION)/$(COMPONENT_ARCHIVE)
COMPONENT_BUGDB =	    $(COMPONENT_FMRI)

COMPONENT_SUMMARY =	High-performance and widely portable implementation of the MPI standard
COMPONENT_LICENSE_FILE = $(COMPONENT_SRC_NAME).license
COMPONENT_LICENSE =	MIT

include $(WS_MAKE_RULES)/prep.mk
include $(WS_MAKE_RULES)/configure.mk
include $(WS_MAKE_RULES)/ips.mk  

CONFIGURE_PREFIX =	$(MPI_PREFIX)

CONFIGURE_OPTIONS+=	--bindir=$(MPI_BINDIR)
CONFIGURE_OPTIONS+=	--datadir=$(MPI_DATADIR)
CONFIGURE_OPTIONS+=	--docdir=$(MPI_DOCDIR)
CONFIGURE_OPTIONS+=	--htmldir=$(MPI_HTMLDIR)
CONFIGURE_OPTIONS+=	--includedir=$(MPI_INCDIR)
CONFIGURE_OPTIONS+=	--libdir=$(MPI_LIBDIR)
CONFIGURE_OPTIONS+=	--mandir=$(MPI_MANDIR)
CONFIGURE_OPTIONS+=	--sbindir=$(MPI_SBINDIR)
CONFIGURE_OPTIONS+=	--sysconfdir=$(MPI_ETCDIR)
CONFIGURE_OPTIONS+=	--with-pm=hydra
CONFIGURE_OPTIONS+=	--with-device=ch3:sock
CONFIGURE_OPTIONS+=	--with-thread-package=posix
CONFIGURE_OPTIONS+=	--enable-shared
CONFIGURE_OPTIONS+=	--enable-rpath
CONFIGURE_OPTIONS+=	--enable-f77
CONFIGURE_OPTIONS+=	--enable-fc
CONFIGURE_OPTIONS+=	--enable-cxx
CONFIGURE_OPTIONS+=	--enable-mpe

# It seems the issue has gone with the 3.x series
#CONFIGURE_OPTIONS  +=	CFLAGS="$(CFLAGS) -Wa,--divide"

COMPONENT_TEST_TARGETS = test

build:		$(BUILD_32_and_64)

$(BUILD_DIR)/modulefile.%:		files/modulefile
	/bin/sed -e "s#%%BITS%%#$*#g" \
	-e "s#%%COMPONENT_FMRI%%#$(COMPONENT_FMRI)#g" \
	-e "s#%%COMPONENT_VERSION%%#$(COMPONENT_VERSION)#g" \
	-e "s#%%MODULE_CONFLICTS%%#$(MPI_IMPLEMENTATIONS_LIST)#g" \
	-e "s#%%MODULE_INCDIR%%#$(MPI_INCDIR)#g" \
	-e "s#%%MODULE_PREFIX%%#$(MPI_PREFIX.$*)#g" < $< > $@ ;

PROTO_MODULE_PATH = $(PROTO_DIR)$(MODULE_PATH)/$(COMPONENT_NAME)

install:	$(INSTALL_32_and_64) $(BUILD_DIR)/modulefile.32 $(BUILD_DIR)/modulefile.64
	$(MKDIR) $(PROTO_MODULE_PATH)/32
	$(CP) $(BUILD_DIR)/modulefile.32 $(PROTO_MODULE_PATH)/32/$(COMPONENT_VERSION)
	$(MKDIR) $(PROTO_MODULE_PATH)/64
	$(CP) $(BUILD_DIR)/modulefile.64 $(PROTO_MODULE_PATH)/64/$(COMPONENT_VERSION)

test:	$(TEST_32)



BUILD_PKG_DEPENDENCIES =	$(BUILD_TOOLS)

include $(WS_MAKE_RULES)/depend.mk
