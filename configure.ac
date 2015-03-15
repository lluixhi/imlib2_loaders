# get rid of that stupid cache mechanism
rm -f config.cache

AC_INIT([imlib2_loaders],[1.4.7],[enlightenment-devel@lists.sourceforge.net])
AC_CONFIG_SRCDIR(configure.ac)
AC_CANONICAL_BUILD
AC_CANONICAL_HOST

AM_INIT_AUTOMAKE(1.6 dist-bzip2)
AC_CONFIG_HEADERS(config.h)

AC_C_BIGENDIAN
AC_PROG_CC
AC_HEADER_STDC
AC_C_CONST
LT_INIT

if test "x${bindir}" = 'x${exec_prefix}/bin'; then
  if test "x${exec_prefix}" = "xNONE"; then
    if test "x${prefix}" = "xNONE"; then
      bindir="${ac_default_prefix}/bin";
    else
      bindir="${prefix}/bin";
    fi
  else
    if test "x${prefix}" = "xNONE"; then
      bindir="${ac_default_prefix}/bin";
    else
      bindir="${prefix}/bin";
    fi
  fi
fi

if test "x${libdir}" = 'x${exec_prefix}/lib'; then
  if test "x${exec_prefix}" = "xNONE"; then
    if test "x${prefix}" = "xNONE"; then
      libdir="${ac_default_prefix}/lib";
    else
      libdir="${prefix}/lib";
    fi
  else
    if test "x${prefix}" = "xNONE"; then
      libdir="${ac_default_prefix}/lib";
    else
      libdir="${prefix}/lib";
    fi
  fi
fi

dnl Set PACKAGE_BIN_DIR in config.h.
if test "x${bindir}" = 'xNONE'; then
  if test "x${prefix}" = "xNONE"; then
    AC_DEFINE_UNQUOTED(PACKAGE_BIN_DIR, "${ac_default_prefix}/bin", [Installation directory for user executables])
  else
    AC_DEFINE_UNQUOTED(PACKAGE_BIN_DIR, "${prefix}/bin", [Installation directory for user executables])
  fi
else
  AC_DEFINE_UNQUOTED(PACKAGE_BIN_DIR, "${bindir}", [Installation directory for user executables])
fi

dnl Set PACKAGE_LIB_DIR in config.h.
if test "x${libdir}" = 'xNONE'; then
  if test "x${prefix}" = "xNONE"; then
    AC_DEFINE_UNQUOTED(PACKAGE_LIB_DIR, "${ac_default_prefix}/lib", [Installation directory for libraries])
  else
    AC_DEFINE_UNQUOTED(PACKAGE_LIB_DIR, "${prefix}/lib", [Installation directory for libraries])
  fi
else
  AC_DEFINE_UNQUOTED(PACKAGE_LIB_DIR, "${libdir}", [Installation directory for libraries])
fi

dnl Set PACKAGE_DATA_DIR in config.h.
if test "x${prefix}" = "xNONE"; then
  AC_DEFINE_UNQUOTED(PACKAGE_DATA_DIR, "${ac_default_prefix}/share/${PACKAGE}", [Shared Data Directory])
else
  AC_DEFINE_UNQUOTED(PACKAGE_DATA_DIR, "${prefix}/share/${PACKAGE}", [Shared Data Directory])
fi

dnl Set PACKAGE_SOURCE_DIR in config.h.
packagesrcdir=`cd $srcdir && pwd`
AC_DEFINE_UNQUOTED(PACKAGE_SOURCE_DIR, "${packagesrcdir}", [Source code directory])

AC_PATH_X
AC_PATH_XTRA

if test "x$have_x" = "xno"; then
   # If --without-x was passed, this will be "disabled" instead of "no" --
   # so always treat "no" as an error
   AC_MSG_ERROR(
[no X support found.  Use --x-includes and --x-libraries to specify the X
path on your system, or --without-x to disable X support.])
fi

if test "x$have_x" = "xyes"; then
  AC_DEFINE(BUILD_X11, 1, [enabling X11 support])
else
  have_x="no"
fi
AM_CONDITIONAL(BUILD_X11, test "x$have_x" = "xyes")

AC_MSG_CHECKING(whether to enable xcf support)
AC_ARG_WITH([xcf],
  [AS_HELP_STRING([--without-xcf], [Disable XCF image loader])],
  [
   if test "$withval" = no ; then
     xcf_loader=no
   else
     xcf_loader=yes
   fi
  ],
  [ xcf_loader=auto ]
)
AC_MSG_RESULT($xcf_loader)
if test "$xcf_loader" != no ; then
    xcf_ok="yes"
else
    xcf_ok="no"
fi
AM_CONDITIONAL(BUILD_XCF_LOADER, test "$xcf_ok" = yes)


AC_MSG_CHECKING(whether to enable png support)
AC_ARG_WITH([png],
  [AS_HELP_STRING([--without-png], [Disable PNG image loader])],
  [
   if test "$withval" = no ; then
     png_loader=no
   else
     png_loader=yes
   fi
  ],
  [ png_loader=auto ]
)
AC_MSG_RESULT($png_loader)

if test "$png_loader" != no ; then
  PKG_CHECK_MODULES(PNG, libpng, [ png_ok="yes" ], [
    PKG_CHECK_MODULES(PNG, libpng12, [ png_ok="yes" ], [
      PKG_CHECK_MODULES(PNG, libpng10, [ png_ok="yes" ], [ png_ok="no"])
    ])
  ])
else
  png_ok="no"
fi
AM_CONDITIONAL(BUILD_PNG_LOADER, test "$png_ok" = yes)


AC_MSG_CHECKING(whether to enable jpeg support)
AC_ARG_WITH([jpeg],
  [AS_HELP_STRING([--without-jpeg], [Disable JPEG image loader])],
  [
   if test "$withval" = no ; then
     jpeg_loader=no
   else
     jpeg_loader=yes
   fi
  ],
  [ jpeg_loader=auto ]
)
AC_MSG_RESULT($jpeg_loader)

if test "$jpeg_loader" != no ; then
  AC_CHECK_LIB(jpeg, jpeg_destroy_decompress, jpeg_ok=yes, jpeg_ok=no)
  if test "$jpeg_ok" = yes; then
    AC_CHECK_HEADER([jpeglib.h], , jpeg_ok=no)
    if test "$jpeg_ok" = yes; then
      JPEGLIBS="-ljpeg"
    fi
  fi
  if test "$jpeg_ok" = no ; then
    if test "$jpeg_loader" = yes ; then
      AC_MSG_ERROR(JPEG support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native JPEG support will not be built (JPEG not found) ***)
    fi
  fi
else
  jpeg_ok=no
fi
AM_CONDITIONAL(BUILD_JPEG_LOADER, test "$jpeg_ok" = yes)
AC_SUBST(JPEGLIBS)


AC_MSG_CHECKING(whether to enable tiff support)
AC_ARG_WITH([tiff],
  [AS_HELP_STRING([--without-tiff], [Disable TIFF image loader])],
  [
   if test "$withval" = no ; then
     tiff_loader=no
   else
     tiff_loader=yes
   fi
  ],
  [ tiff_loader=auto ]
)
AC_MSG_RESULT($tiff_loader)

if test "$tiff_loader" != no ; then
  AC_CHECK_LIB(tiff, TIFFReadScanline,
    tiff_libs="-ltiff"
    tiff_ok=yes,
  AC_CHECK_LIB(tiff, TIFFReadScanline,
    tiff_libs="-ltiff -ljpeg -lz -lm"
    tiff_ok=yes,
  AC_CHECK_LIB(tiff34, TIFFReadScanline,
    tiff_libs="-ltiff34 -ljpeg -lz -lm"
    tiff_ok=yes,
    tiff_ok=no,
    -ljpeg -lz -lm),
    -ljpeg -lz -lm),
    )
  if test "$tiff_ok" = yes; then
    AC_CHECK_HEADER([tiffio.h], , tiff_ok=no)
    if test "$tiff_ok" = yes; then
      TIFFLIBS=$tiff_libs
    fi
  fi
  if test "$tiff_ok" = no ; then
    if test "$tiff_loader" = yes ; then
      AC_MSG_ERROR(TIFF support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native TIFF support will not be built (TIFF not found) ***)
    fi
  fi
else
  tiff_ok=no
fi
AM_CONDITIONAL(BUILD_TIFF_LOADER, test "$tiff_ok" = yes)
AC_SUBST(TIFFLIBS)


AC_MSG_CHECKING(whether to enable gif support)
gif_lib=any
AC_ARG_WITH([gif],
  [AS_HELP_STRING([--without-gif], [Disable GIF image loader])],
  [
   if test "$withval" = no ; then
     gif_loader=no
   else
     gif_loader=yes
     case "$withval" in
       giflib)   gif_lib=giflib;;
       libungif) gif_lib=libungif;;
     esac
   fi
  ],
  [ gif_loader=auto ]
)
AC_MSG_RESULT($gif_loader)

if test "$gif_loader" != no ; then
  AC_MSG_CHECKING(for preferred gif provider)
  AC_MSG_RESULT($gif_lib)

  if test "$gif_lib" != libungif ; then
    AC_CHECK_LIB(gif, DGifOpenFileName, gif_libs="-lgif" gif_ok=yes, gif_ok=no)
    if test "$gif_ok" = yes; then
      AC_CHECK_HEADER([gif_lib.h], , gif_ok=no)
      if test "$gif_ok" = yes; then
        GIFLIBS=$gif_libs
        gif_lib=giflib
      fi
    fi
  fi

  if test "$gif_lib" != giflib ; then
    AC_CHECK_LIB(ungif, DGifOpenFileName, gif_libs="-lungif" gif_ok=yes, gif_ok=no)
    if test "$gif_ok" = yes; then
      AC_CHECK_HEADER([gif_lib.h], , gif_ok=no)
      if test "$gif_ok" = yes; then
        GIFLIBS=$gif_libs
        gif_lib=libungif
      fi
    fi
  fi

  if test "$gif_ok" = no ; then
    if test "$gif_loader" = yes ; then
      AC_MSG_ERROR(GIF support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native GIF support will not be built (GIF not found) ***)
    fi
  fi
else
  gif_ok=no
fi
AM_CONDITIONAL(BUILD_GIF_LOADER, test "$gif_ok" = yes)
AC_SUBST(GIFLIBS)


AC_MSG_CHECKING(whether to enable zlib support)
AC_ARG_WITH([zlib],
  [AS_HELP_STRING([--without-zlib], [Disable ZLIB loader])],
  [
   if test "$withval" = no ; then
     zlib_loader=no
   else
     zlib_loader=yes
   fi
  ],
  [ zlib_loader=auto ]
)
AC_MSG_RESULT($zlib_loader)

if test "$zlib_loader" != no ; then
  AC_CHECK_LIB(z, uncompress, zlib_ok=yes, zlib_ok=no)
  if test "$zlib_ok" = yes; then
    AC_CHECK_HEADER([zlib.h], , zlib_ok=no)
    if test "$zlib_ok" = yes; then
      ZLIBLIBS="-lz"
    fi
  fi
  if test "$zlib_ok" = no ; then
    if test "$zlib_loader" = yes ; then
      AC_MSG_ERROR(ZLIB support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native zlib support will not be built (zlib not found) ***)
    fi
  fi
else
  zlib_ok=no
fi
AM_CONDITIONAL(BUILD_ZLIB_LOADER, test "$zlib_ok" = yes)
AC_SUBST(ZLIBLIBS)


AC_MSG_CHECKING(whether to enable bzip2 support)
AC_ARG_WITH([bzip2],
  [AS_HELP_STRING([--without-bzip2], [Disable BZIP2 loader])],
  [
   if test "$withval" = no ; then
     bz2_loader=no
   else
     bz2_loader=yes
   fi
  ],
  [ bz2_loader=auto ]
)
AC_MSG_RESULT($bz2_loader)

if test "$bz2_loader" != no ; then
  AC_CHECK_LIB(bz2, BZ2_bzRead, bz2_ok=yes, bz2_ok=no)
  if test "$bz2_ok" = yes; then
    AC_CHECK_HEADER([bzlib.h], , bz2_ok=no)
    if test "$bz2_ok" = yes; then
      BZ2LIBS="-lbz2"
    fi
  fi
  if test "$bz2_ok" = no ; then
    if test "$bz2_loader" = yes ; then
      AC_MSG_ERROR(BZIP2 support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native bz2 support will not be built (bzip2 not found) ***)
    fi
  fi
else
  bz2_ok=no
fi
AM_CONDITIONAL(BUILD_BZ2_LOADER, test "$bz2_ok" = yes)
AC_SUBST(BZ2LIBS)


AC_MSG_CHECKING(whether to enable id3 support)
AC_ARG_WITH([id3],
  [AS_HELP_STRING([--without-id3], [Disable ID3 loader])],
  [
   if test "$withval" = no ; then
     id3_loader=no
   else
     id3_loader=yes
   fi
  ],
  [ id3_loader=auto ]
)
AC_MSG_RESULT($id3_loader)

if test "$id3_loader" != no ; then
  AC_CHECK_LIB(id3tag, id3_file_open,
    id3_libs="-lz -lid3tag"
    id3_ok=yes,
    id3_ok=no,
    -lz)
  if test "$id3_ok" = yes; then
    AC_CHECK_HEADER([id3tag.h], , id3_ok=no)
    if test "$id3_ok" = yes; then
      ID3LIBS=$id3_libs
    fi
  fi
  if test "$id3_ok" = no ; then
    if test "$id3_loader" = yes ; then
      AC_MSG_ERROR(ID3 support was requested but system does not support it)
    else
      AC_MSG_WARN(*** Native mp3 support will not be built (id3tag not found) ***)
    fi
  fi
else
  id3_ok=no
fi
AM_CONDITIONAL(BUILD_ID3_LOADER, test "$id3_ok" = yes)
AC_SUBST(ID3LIBS)


AC_ARG_WITH(imlib2-config, [  --with-imlib2-config=IMLIB2_CONFIG    use imlib2-config specified ],
[ IMLIB2_CONFIG=$withval;
  echo "using "$IMLIB2_CONFIG" for imlib2-config"; ],
[ if test -z "$IMLIB2_CONFIG"; then
    AC_PATH_PROG(IMLIB2_CONFIG, "imlib2-config", "", $PATH)
  fi
])
my_includes=`$IMLIB2_CONFIG --cflags`

AC_SUBST(my_includes)

AC_CONFIG_FILES([
Makefile
src/Makefile
src/modules/Makefile
src/modules/loaders/Makefile
README
imlib2_loaders.spec
])
AC_CONFIG_COMMANDS([default],[
],[])
AC_OUTPUT

#####################################################################
## Info

echo
echo
echo
echo "------------------------------------------------------------------------"
echo "$PACKAGE $VERSION"
echo "------------------------------------------------------------------------"
echo
echo "Configuration Options Summary:"
echo
echo
echo "Image Loaders:"
echo "  JPEG....................: $jpeg_ok"
echo "  PNG.....................: $png_ok"
echo "  TIFF....................: $tiff_ok"
echo "  GIF.....................: $gif_ok"
echo "  ZLIB....................: $zlib_ok"
echo "  BZIP2...................: $bz2_ok"
echo "  ID3.....................: $id3_ok"
echo "  XCF.....................: $xcf_ok"
echo "  X.......................: $have_x"
echo
