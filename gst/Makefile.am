## Process this file with automake to produce Makefile.in

CLEANFILES =

AM_CPPFLAGS = \
    -DPACKAGE_LOCALE_DIR=\""$(localedir)"\" \
    -DPACKAGE_SRC_DIR=\""$(srcdir)"\" \
    -DPACKAGE_DATA_DIR=\""$(pkgdatadir)"\" \
    $(GLIB_CFLAGS) \
    $(GSTREAMER_CFLAGS) \
    -I$(top_srcdir)/owr \
    -I$(top_srcdir)/local

AM_CFLAGS = \
    -Wall \
    -Wextra \
    -Werror \
    -Wno-parentheses-equality \
    $(DEBUG_CFLAGS)

lib_LTLIBRARIES = libopenwebrtc_gst.la

libopenwebrtc_gst_la_SOURCES = \
    owr_gst_audio_renderer.c \
    owr_gst_media_source.c \
    owr_gst_video_renderer.c

libopenwebrtc_gst_la_LIBADD = \
    $(top_builddir)/owr/libopenwebrtc.la \
    $(GSTREAMER_LIBS) \
    $(GLIB_LIBS)

libopenwebrtc_gst_la_DEPENDENCIES = \
    $(top_builddir)/owr/libopenwebrtc.la

includedir = $(prefix)/include/owr
include_HEADERS = \
    owr_gst_audio_renderer.h \
    owr_gst_media_source.h \
    owr_gst_video_renderer.h

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = openwebrtc-gst-@OWR_API_VERSION@.pc

EXTRA_DIST = \
    openwebrtc-gst-@OWR_API_VERSION@.pc.in

-include $(INTROSPECTION_MAKEFILE)
INTROSPECTION_GIRS =

INTROSPECTION_SCANNER_ARGS = --add-include-path=$(srcdir) --add-include-path=$(srcdir)/../owr --warn-all
if !OWR_STATIC
INTROSPECTION_SCANNER_ARGS += --pkg gstreamer-1.0 --pkg gstreamer-video-1.0 --pkg gstreamer-app-1.0
endif

INTROSPECTION_COMPILER_ARGS = --includedir=$(srcdir)
if OWR_STATIC
INTROSPECTION_COMPILER_ARGS += --shared-library=""
endif

if HAVE_INTROSPECTION
introspection_sources = $(libopenwebrtc_gst_la_SOURCES)

OwrGst-@OWR_API_VERSION@.gir: libopenwebrtc_gst.la
OwrGst_@OWR_API_VERSION_UNDERSCORE@_gir_INCLUDES = GObject-2.0 Gst-1.0 Owr-@OWR_API_VERSION@
OwrGst_@OWR_API_VERSION_UNDERSCORE@_gir_CFLAGS = \
    $(INCLUDES) \
    -I$(top_srcdir)/owr \
    -I$(top_srcdir)/local
OwrGst_@OWR_API_VERSION_UNDERSCORE@_gir_LIBS = libopenwebrtc_gst.la
OwrGst_@OWR_API_VERSION_UNDERSCORE@_gir_FILES = \
    owr_gst_audio_renderer.c \
    owr_gst_audio_renderer.h \
    owr_gst_media_source.c \
    owr_gst_media_source.h \
    owr_gst_video_renderer.c \
    owr_gst_video_renderer.h

INTROSPECTION_GIRS += OwrGst-@OWR_API_VERSION@.gir

girdir = $(datadir)/gir-1.0
gir_DATA = $(INTROSPECTION_GIRS)

typelibdir = $(libdir)/girepository-1.0
typelib_DATA = $(INTROSPECTION_GIRS:.gir=.typelib)

OwrGst-@OWR_API_VERSION@.gir.h: OwrGst-@OWR_API_VERSION@.gir
	xxd -i $< > $@
	shasum -a 1 -b < $< | head -c 40 > $<.sha1
	xxd -i $<.sha1 >> $@
	rm -f $<.sha1

CLEANFILES += $(gir_DATA) $(typelib_DATA) OwrGst-@OWR_API_VERSION@.gir.h
else
OwrGst-@OWR_API_VERSION@.gir.h:
	cp -f `find $(top_srcdir) -name OwrGst-@OWR_API_VERSION@.gir.h || echo "OwrGst-@OWR_API_VERSION@.gir.h"` .
endif

-include $(top_srcdir)/git.mk
