MAINTAINERCLEANFILES = Makefile.in

AM_CPPFLAGS          = -I. \
                       -I$(top_srcdir) \
                       -I$(top_srcdir)/src/modules/loaders \
                       @my_includes@

pkgdir               = $(libdir)/imlib2/loaders

if BUILD_XCF_LOADER
XCF_L                = xcf.la
endif

pkg_LTLIBRARIES      = \
$(XCF_L) \
ico.la \
ani.la

xcf_la_SOURCES      = loader_xcf.c loader_xcf_pixelfuncs.c image.h common.h color_values.h
xcf_la_LDFLAGS      = -module -avoid-version
xcf_la_LIBADD       =
xcf_la_LIBTOOLFLAGS = --tag=disable-static

ico_la_SOURCES      = loader_ico.c common.h image.h
ico_la_LDFLAGS      = -module -avoid-version
ico_la_LIBADD       =
ico_la_LIBTOOLFLAGS = --tag=disable-static

ani_la_SOURCES      = loader_ani.c common.h image.h
ani_la_LDFLAGS      = -module -avoid-version
ani_la_LIBADD       =
ani_la_LIBTOOLFLAGS = --tag=disable-static
