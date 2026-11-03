ACLOCAL_AMFLAGS = -I m4

SUBDIRS = src
EXTRA_DIST = \
    CMakeLists.txt \
    src/CMakeLists.txt \
    src/test/CMakeLists.txt \
    src/test/testcommops.c \
    src/test/testsplit.c \
    src/test/testsplit2.c \
    cmake/lwgrpConfig.cmake.in

dist_pkgdata_DATA = README LICENSE.TXT

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = liblwgrp.pc
