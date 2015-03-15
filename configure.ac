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


AC_ARG_ENABLE(xcf,
[  --disable-xcf           disable building the xcf loader],
[
 if test x$enableval = xno; then
   xcf=no;
 fi
])


AC_ARG_WITH(imlib2-config, [  --with-imlib2-config=IMLIB2_CONFIG    use imlib2-config specified ],
[ IMLIB2_CONFIG=$withval;
  echo "using "$IMLIB2_CONFIG" for imlib2-config"; ],
[ if test -z "$IMLIB2_CONFIG"; then
    AC_PATH_PROG(IMLIB2_CONFIG, "imlib2-config", "", $PATH)
  fi
])
my_includes=`$IMLIB2_CONFIG --cflags`

AC_SUBST(my_includes)

AM_CONDITIONAL(BUILD_XCF_LOADER, test x$xcf != xno)

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

echo -e "\nBuild summary:"
echo    "------------------------"
echo -n " XCF: "
if test x$xcf = xno ; then
  echo    "Disabled "
else
  echo    "Enabled "
fi
