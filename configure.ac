#
# $Id$
#
# Copyright 2011-2012, Juniper Networks, Inc.
# All rights reserved.
# This SOFTWARE is licensed under the LICENSE provided in the
# ../Copyright file. By downloading, installing, copying, or otherwise
# using the SOFTWARE, you agree to be bound by the terms of that
# LICENSE.

AC_PREREQ(2.2)
AC_INIT([yangc], [0.0.1], [phil@juniper.net])
AM_INIT_AUTOMAKE([-Wall -Werror foreign -Wno-portability])

case $prefix in
     NONE)
	prefix=/usr/local
	;;
esac

case $exec_prefix in
     NONE)
	exec_prefix=$prefix
	;;
esac

# Support silent build rules.  Requires at least automake-1.11.
# Disable with "configure --disable-silent-rules" or "make V=1"
m4_ifdef([AM_SILENT_RULES], [AM_SILENT_RULES([yes])])

#
# libxslt 1.1.26 has a fix for AVT terminating with close braces
#
LIBXML_REQUIRED_VERSION=2.7.7
LIBXSLT_REQUIRED_VERSION=1.1.26
LIBSLAX_REQUIRED_VERSION=0.12.3

AC_PROG_CC
AM_PROG_AR
AC_PROG_INSTALL
AC_CONFIG_MACRO_DIR([m4])

# Must be after AM_PROG_AR
LT_INIT([dlopen])

AC_PATH_PROG(BASENAME, basename, /usr/bin/basename)
AC_PATH_PROG(BISON, bison, /usr/bin/bison)
AC_PATH_PROG(CAT, cat, /bin/cat)
AC_PATH_PROG(CHMOD, chmod, /bin/chmod)
AC_PATH_PROG(CP, cp, /bin/cp)
AC_PATH_PROG(DIFF, diff, /usr/bin/diff)
AC_PATH_PROG(MKDIR, mkdir, /bin/mkdir)
AC_PATH_PROG(MV, mv, /bin/mv)
AC_PATH_PROG(RM, rm, /bin/rm)
AC_PATH_PROG(SED, sed, /bin/sed)

AC_PROG_LN_S

AC_STDC_HEADERS

# Checks for typedefs, structures, and compiler characteristics.
AC_C_INLINE
AC_TYPE_SIZE_T

# Checks for library functions.
AC_FUNC_ALLOCA
AC_FUNC_MALLOC
AC_FUNC_REALLOC
AC_CHECK_FUNCS([bzero memmove strchr strcspn strerror strspn])
AC_CHECK_FUNCS([sranddev srand strlcpy])
AC_CHECK_FUNCS([fdopen getrusage])
AC_CHECK_FUNCS([gettimeofday ctime])
AC_CHECK_FUNCS([getpass])
AC_CHECK_FUNCS([sysctlbyname])
AC_CHECK_FUNCS([flock])
AC_CHECK_FUNCS([statfs])
AC_CHECK_FUNCS([strnstr])
AC_CHECK_FUNCS([strndup])

AC_CHECK_HEADERS([sys/time.h])
AC_CHECK_HEADERS([ctype.h errno.h stdio.h stdlib.h])
AC_CHECK_HEADERS([string.h sys/param.h unistd.h])
AC_CHECK_HEADERS([sys/sysctl.h])
AC_CHECK_HEADERS([stdint.h sys/statfs.h])


AC_CHECK_LIB([crypto], [MD5_Init])
AM_CONDITIONAL([HAVE_LIBCRYPTO], [test "$HAVE_LIBCRYPTO" != "no"])

AC_CHECK_LIB([m], [lrint])
AM_CONDITIONAL([HAVE_LIBM], [test "$HAVE_LIBM" != "no"])

AC_CHECK_LIB([xml2], [xmlNewParserCtxt])
AC_CHECK_LIB([xslt], [xsltInit])
AC_CHECK_LIB([readline], [readline])

AC_CHECK_MEMBER([struct passwd.pw_class],
    [HAVE_PWD_CLASS=yes ;
        AC_DEFINE([HAVE_PWD_CLASS], [1], [Have struct pwd.pw_class])],
    [HAS_PWD_CLASS=no], [[#include <pwd.h>]])

AC_CHECK_MEMBER([struct sockaddr_un.sun_len],
    [HAVE_SUN_LEN=yes ;
        AC_DEFINE([HAVE_SUN_LEN], [1], [Have struct sockaddr_un.sun_len])],
    [HAS_SUN_LEN=no], [[#include <sys/un.h>]])

dnl
dnl Some packages need to be checked against version numbers so we
dnl define a function here for later use
dnl
AC_DEFUN([VERSION_TO_NUMBER],
[`$1 | sed -e 's/lib.* //' | awk 'BEGIN { FS = "."; } { printf "%d", ([$]1 * 1000 + [$]2) * 1000 + [$]3;}'`])

AC_MSG_CHECKING([whether to build with warnings])
AC_ARG_ENABLE([warnings],
    [  --enable-warnings     Turn on compiler warnings],
    [YANGC_WARNINGS=$enableval],
    [YANGC_WARNINGS=no])
AC_MSG_RESULT([$YANGC_WARNINGS])
AM_CONDITIONAL([YANGC_WARNINGS_HIGH], [test "$YANGC_WARNINGS" != "no"])

AC_MSG_CHECKING([whether to build CLIRA web interface])
AC_ARG_ENABLE([clira],
    [  --enable-clira        Build the CLIRA web interface],
    [NEED_CLIRA=$enableval],
    [NEED_CLIRA=no])
AC_MSG_RESULT([$NEED_CLIRA])
AM_CONDITIONAL([NEED_CLIRA], [test "$NEED_CLIRA" != "no"])
AC_SUBST(NEED_CLIRA)

AC_MSG_CHECKING([whether to build mixer])
AC_ARG_ENABLE([mixer],
    [  --enable-mixer        Build the mixer binary],
    [NEED_MIXER=$enableval],
    [NEED_MIXER=$NEED_CLIRA])
AC_MSG_RESULT([$NEED_MIXER])
AM_CONDITIONAL([NEED_MIXER], [test "$NEED_MIXER" != "no"])
AC_SUBST(NEED_MIXER)

AC_MSG_CHECKING([whether to build with debugging])
AC_ARG_ENABLE([debug],
    [  --enable-debug        Turn on debugging],
    [YANGC_DEBUG=$enable_debug],
    [YANGC_DEBUG=no])
AC_MSG_RESULT([$YANGC_DEBUG])
AM_CONDITIONAL([YANGC_DEBUG], [test "$YANGC_DEBUG" = "yes"])
if test "YANGC_DEBUG" = "yes" ; then
    AC_DEFINE([YANGC_DEBUG], [1], [Enable debugging])
fi
AC_SUBST(YANGC_DEBUG)

PATH_YANGC=`eval echo $bindir/yangc`
AC_DEFINE_UNQUOTED(PATH_YANGC, ["$PATH_YANGC"], [Path to yangc binary])

AC_MSG_CHECKING([whether to build with readline])
AC_ARG_ENABLE([readline],
    [  --enable-readline    Enable support for GNU readline],
    [HAVE_READLINE=$enable_readline],
    [HAVE_READLINE=$ac_cv_lib_readline_readline])
AC_MSG_RESULT([$HAVE_READLINE])
AM_CONDITIONAL([HAVE_READLINE], [test "$HAVE_READLINE" = "yes"])
if test "$HAVE_READLINE" = "yes" ; then
    AC_DEFINE([HAVE_READLINE], [1], [Enable support for GNU readline])
fi

AC_MSG_CHECKING([whether to build with libedit])
AC_ARG_ENABLE([libedit],
    [  --enable-libedit    Enable support for libedit (BSD readline)],
    [HAVE_LIBEDIT=yes; AC_DEFINE([HAVE_LIBEDIT], [1], [Support libedit])],
    [HAVE_LIBEDIT=no])
AC_MSG_RESULT([$HAVE_LIBEDIT])
AM_CONDITIONAL([HAVE_LIBEDIT], [test "$HAVE_LIBEDIT" != "no"])

AC_MSG_CHECKING([whether to build with __printflike])
AC_ARG_ENABLE([printflike],
    [  --enable-printflike    Enable use of GCC __printflike attribute],
    [HAVE_PRINTFLIKE=$enable_printflike],
    [HAVE_PRINTFLIKE=no])
AC_MSG_RESULT([$HAVE_PRINTFLIKE])
AM_CONDITIONAL([HAVE_PRINTFLIKE], [test "$HAVE_PRINTFLIKE" = "yes"])
if test "$HAVE_PRINTFLIKE" = "yes" ; then
    AC_DEFINE([HAVE_PRINTFLIKE], [1], [Enable use of GCC __printflike])
fi

#
# ---- start of noise
#

#
# ---- handle libxml2
#

LIBXML_CONFIG_PREFIX=""
LIBXML_SRC=""

AC_ARG_WITH(libxml-prefix,
        [  --with-libxml-prefix=[PFX]           Specify location of libxml config],
        LIBXML_CONFIG_PREFIX=$withval
)
        
AC_ARG_WITH(libxml-include-prefix,
        [  --with-libxml-include-prefix=[PFX]   Specify location of libxml headers],
        LIBXML_CFLAGS="-I$withval"
)

AC_ARG_WITH(libxml-libs-prefix,
        [  --with-libxml-libs-prefix=[PFX]      Specify location of libxml libs],
        LIBXML_LIBS="-L$withval"
)

AC_ARG_WITH(libxml-src,
        [  --with-libxml-src=[DIR]              For libxml thats not installed yet (sets all three above)],
        LIBXML_SRC="$withval"
)
AC_SUBST(LIBXML_SRC)

dnl
dnl where is xml2-config
dnl

AC_SUBST(LIBXML_REQUIRED_VERSION)
AC_MSG_CHECKING(for libxml libraries >= $LIBXML_REQUIRED_VERSION) 
if test "x$LIBXML_CONFIG_PREFIX" != "x"
then
        XML_CONFIG=${LIBXML_CONFIG_PREFIX}/bin/xml2-config
else
        XML_CONFIG=xml2-config
fi

dnl
dnl make sure xml2-config is executable,
dnl test version and init our variables
dnl

if ${XML_CONFIG} --libs > /dev/null 2>&1
then
	LIBXML_VERSION=`$XML_CONFIG --version`
	if test VERSION_TO_NUMBER(echo $LIBXML_VERSION) -ge VERSION_TO_NUMBER(echo $LIBXML_REQUIRED_VERSION)
	then
		LIBXML_LIBS="$LIBXML_LIBS `$XML_CONFIG --libs`"
		LIBXML_CFLAGS="$LIBXML_CFLAGS `$XML_CONFIG --cflags`"
		AC_MSG_RESULT($LIBXML_VERSION found)
	else
		AC_MSG_ERROR(Version $LIBXML_VERSION found. You need at least libxml2 $LIBXML_REQUIRED_VERSION for this version of libxslt)
	fi
else
	AC_MSG_ERROR([Could not find libxml2 anywhere, check ftp://xmlsoft.org/.])
fi

AC_SUBST(XML_CONFIG)
AC_SUBST(LIBXML_LIBS)
AC_SUBST(LIBXML_CFLAGS)



#
# ---- handle libxslt
#

LIBXSLT_CONFIG_PREFIX=""
LIBXSLT_SRC=""

AC_ARG_WITH(libxslt-prefix,
        [  --with-libxslt-prefix=[PFX]           Specify location of libxslt config],
        LIBXSLT_CONFIG_PREFIX=$withval
)
        
AC_ARG_WITH(libxslt-include-prefix,
        [  --with-libxslt-include-prefix=[PFX]   Specify location of libxslt headers],
        LIBXSLT_CFLAGS="-I$withval"
)

AC_ARG_WITH(libxslt-libs-prefix,
        [  --with-libxslt-libs-prefix=[PFX]      Specify location of libxslt libs],
        LIBXSLT_LIBS="-L$withval"
)

AC_ARG_WITH(libxslt-src,
        [  --with-libxslt-src=[DIR]              For libxslt thats not installed yet (sets all three above)],
        LIBXSLT_SRC="$withval"
)
AC_SUBST(LIBXSLT_SRC)

dnl
dnl where is xslt-config
dnl

AC_SUBST(LIBXSLT_REQUIRED_VERSION)
AC_MSG_CHECKING(for libxslt libraries >= $LIBXSLT_REQUIRED_VERSION) 
if test "x$LIBXSLT_CONFIG_PREFIX" != "x"
then
        XSLT_CONFIG=${LIBXSLT_CONFIG_PREFIX}/bin/xslt-config
else
        XSLT_CONFIG=xslt-config
fi

dnl
dnl make sure xslt-config is executable,
dnl test version and init our variables
dnl

if ${XSLT_CONFIG} --libs > /dev/null 2>&1
then
	LIBXSLT_VERSION=`$XSLT_CONFIG --version`
	if test VERSION_TO_NUMBER(echo $LIBXSLT_VERSION) -ge VERSION_TO_NUMBER(echo $LIBXSLT_REQUIRED_VERSION)
	then
		LIBXSLT_LIBS="$LIBXSLT_LIBS `$XSLT_CONFIG --libs`"
		LIBXSLT_CFLAGS="$LIBXSLT_CFLAGS `$XSLT_CONFIG --cflags`"
		AC_MSG_RESULT($LIBXSLT_VERSION found)
	else
		AC_MSG_ERROR(Version $LIBXSLT_VERSION found. You need at least libxslt $LIBXSLT_REQUIRED_VERSION for this version of libxslt)
	fi
else
	AC_MSG_ERROR([Could not find libxslt anywhere, check ftp://xmlsoft.org/.])
fi

AC_SUBST(XSLT_CONFIG)
AC_SUBST(LIBXSLT_LIBS)
AC_SUBST(LIBXSLT_CFLAGS)



#
# ---- handle libslax
#

LIBSLAX_CONFIG_PREFIX=""
LIBSLAX_SRC=""

AC_ARG_WITH(libslax-prefix,
        [  --with-libslax-prefix=[PFX]           Specify location of libslax config],
        LIBSLAX_CONFIG_PREFIX=$withval
)
        
AC_ARG_WITH(libslax-include-prefix,
        [  --with-libslax-include-prefix=[PFX]   Specify location of libslax headers],
        LIBSLAX_CFLAGS="-I$withval"
)

AC_ARG_WITH(libslax-libs-prefix,
        [  --with-libslax-libs-prefix=[PFX]      Specify location of libslax libs],
        LIBSLAX_LIBS="-L$withval"
)

AC_ARG_WITH(libslax-src,
        [  --with-libslax-src=[DIR]              For libslax thats not installed yet (sets all three above)],
        LIBSLAX_SRC="$withval"
)
AC_SUBST(LIBSLAX_SRC)

dnl
dnl where is slax-config
dnl

AC_SUBST(LIBSLAX_REQUIRED_VERSION)
AC_MSG_CHECKING(for libslax libraries >= $LIBSLAX_REQUIRED_VERSION) 
if test "x$LIBSLAX_CONFIG_PREFIX" != "x"
then
        SLAX_CONFIG=${LIBSLAX_CONFIG_PREFIX}/bin/slax-config
else
        SLAX_CONFIG=slax-config
fi

dnl
dnl make sure slax-config is executable,
dnl test version and init our variables
dnl

if ${SLAX_CONFIG} --libs > /dev/null 2>&1
then
	LIBSLAX_VERSION=`$SLAX_CONFIG --version`
	if test VERSION_TO_NUMBER(echo $LIBSLAX_VERSION) -ge VERSION_TO_NUMBER(echo $LIBSLAX_REQUIRED_VERSION)
	then
		LIBSLAX_LIBS="$LIBSLAX_LIBS `$SLAX_CONFIG --libs`"
		LIBSLAX_CFLAGS="$LIBSLAX_CFLAGS `$SLAX_CONFIG --cflags-internal`"
		LIBSLAX_INTERNALDIR="`$SLAX_CONFIG --internal`"
		SLAX_EXTDIR="`$SLAX_CONFIG --extdir | head -1`"
		SLAX_LIBDIR="`$SLAX_CONFIG --libdir | head -1`"
		SLAX_BINDIR="`$SLAX_CONFIG --bindir | head -1`"
		SLAX_OXTRADOCDIR="`$SLAX_CONFIG --oxtradoc | head -1`"
		AC_MSG_RESULT($LIBSLAX_VERSION found)
	else
		AC_MSG_ERROR(Version $LIBSLAX_VERSION found. You need at least libslax $LIBSLAX_REQUIRED_VERSION for this version of yangc)
	fi
else
	AC_MSG_ERROR([Could not find libslax anywhere, check https://github.com/Juniper/libslax.])
fi

AC_SUBST(SLAX_CONFIG)
AC_SUBST(LIBSLAX_CFLAGS)
AC_SUBST(LIBSLAX_INTERNALDIR)
AC_SUBST(LIBSLAX_LIBS)
AC_SUBST(SLAX_BINDIR)
AC_SUBST(SLAX_EXTDIR)
AC_SUBST(SLAX_LIBDIR)
AC_SUBST(SLAX_OXTRADOCDIR)

AC_CHECK_TYPES(socklen_t,,,[#include <sys/types.h>
#include <sys/socket.h>])


#
# Find the version of BISON
#
AC_MSG_CHECKING([bison version])
test -z "${BISON}" && BISON=bison
BISON_VERSION=[`${BISON} --version | grep 'GNU Bison' | cut -d ' ' -f 4 \
    | sed -e 's/[-\.]/ /g' | tr -d a-z \
    | sed -e 's/\([0-9]*\) \([0-9]*\).*/\1.\2/'`]
BISON_VERSION_NUMBER=VERSION_TO_NUMBER(echo $BISON_VERSION)
AC_SUBST(BISON_VERSION)
AC_SUBST(BISON_VERSION_NUMBER)
AM_CONDITIONAL([HAVE_BISON30], [test "$BISON_VERSION_NUMBER" -ge 3000000])
if test "$BISON_VERSION_NUMBER" -ge 3000000 ; then
    AC_DEFINE([HAVE_BISON30], [1], [Bison version >= 3.0])
fi
AC_MSG_RESULT($BISON_VERSION)

#
# ---- end of noise
#

# Cygwin lacks the modern st_mtimespec field in struct stat
AC_MSG_CHECKING([for stat.st_mtimespec])
AC_COMPILE_IFELSE(
    [AC_LANG_PROGRAM([[
#include <sys/types.h>
#include <sys/stat.h>
]],
                     [[struct stat st; st.st_mtimespec.tv_sec = 0;]])],
    [
        AC_MSG_RESULT([yes])
        AC_DEFINE_UNQUOTED([HAVE_MTIMESPEC], 1,
	                   [Define to 1 if you have the `st_mtimespec' field.])
    ],
    [
        AC_MSG_RESULT([no])
        AC_DEFINE_UNQUOTED([HAVE_MTIMESPEC], 0,
                           [Define to 1 if you have the `st_mtimespec' field.])
    ]
)

case $host_os in
     darwin*)
        LIBTOOL=glibtool
	SLAX_LIBEXT=dylib
	;;
     Linux*|linux*)
        CFLAGS="-D_GNU_SOURCE $CFLAGS"
	LDFLAGS=-ldl
	SLAX_LIBEXT=so
	;;
     cygwin*|CYGWIN*)
	LDFLAGS=-no-undefined
	SLAX_LIBEXT=ddl
	;;
esac

YANGCDIR=""

AC_ARG_WITH(yangc-dir,
        [  --with-yangc-dir=[DIR]           Specify location of yangc files],
        [YANGC_DIR=$withval],
        [YANGC_DIR=$prefix/share/yangc]
)
AC_SUBST(YANGC_DIR)

YANGC_LIBS=-lyang
AC_SUBST(YANGC_LIBS)
YANGC_SRCDIR='${srcdir}'
AC_SUBST(YANGC_SRCDIR)
YANGC_LIBDIR='${libdir}'
AC_SUBST(YANGC_LIBDIR)
YANGC_INCLUDEDIR='${includedir}'
AC_SUBST(YANGC_LIBEXECDIR)
YANGC_LIBEXECDIR='${pkglibexecdir}'
AC_SUBST(YANGC_INCLUDEDIR)

AC_SUBST(SLAX_LIBEXT)

dnl for the spec file
RELDATE=`date +'%Y-%m-%d%n'`
AC_SUBST(RELDATE)
UNAME=`uname -a`

AC_MSG_RESULT(Using configure dir $ac_abs_confdir)

if test -d $ac_abs_confdir/.git ; then
  extra=`git branch | awk '/\*/ { print $2 }'`
  if test "$extra" != "" -a "$extra" != "master"
  then
      YANGC_VERSION_EXTRA="-git-$extra"
  fi
fi

YANGC_VERSION=$PACKAGE_VERSION
YANGC_VERSION_NUMBER=VERSION_TO_NUMBER(echo $PACKAGE_VERSION)
AC_SUBST(YANGC_VERSION)
AC_SUBST(YANGC_VERSION_NUMBER)
AC_SUBST(YANGC_VERSION_EXTRA)

AC_DEFINE_UNQUOTED(YANGC_VERSION, ["$YANGC_VERSION"],
    [Version number as dotted value])
AC_DEFINE_UNQUOTED(YANGC_VERSION_NUMBER, [$YANGC_VERSION_NUMBER],
    [Version number as a number])
AC_DEFINE_UNQUOTED(YANGC_VERSION_STRING, ["$YANGC_VERSION_NUMBER"],
    [Version number as string])
AC_DEFINE_UNQUOTED(YANGC_VERSION_EXTRA, ["$YANGC_VERSION_EXTRA"],
    [Version number extra information])
AC_DEFINE_UNQUOTED(YANGC_DIR, ["$YANGC_DIR"],
    [Directory for YANGC shared files])

AC_CONFIG_HEADERS([libyang/yangconfig.h])
AC_CONFIG_FILES([
  Makefile
  yangc-config
  libyang/Makefile
  libyang/yangversion.h
  yangc/Makefile
  packaging/yangc.pc
])
AC_OUTPUT


AC_MSG_NOTICE([summary of build options:

  build system:     ${UNAME}

  yangc version:    ${VERSION} ${YANGC_VERSION_EXTRA}
  host type:        ${host}
  install prefix:   ${prefix}
  compiler:         ${CC}
  compiler flags:   ${CFLAGS}
  library types:    Shared=${enable_shared}, Static=${enable_static}

  libxml version:   ${LIBXML_VERSION}
  libxml cflags:    ${LIBXML_CFLAGS}
  libxml libs:      ${LIBXML_LIBS}

  libxslt version:  ${LIBXSLT_VERSION}
  libxslt cflags:   ${LIBXSLT_CFLAGS}
  libxslt libs:     ${LIBXSLT_LIBS}

  libslax version:  ${LIBSLAX_VERSION}
  libslax cflags:   ${LIBSLAX_CFLAGS}
  libslax headers:  ${LIBSLAX_INTERNALDIR}
  libslax libs:     ${LIBSLAX_LIBS}
  libslax libdir:   ${SLAX_LIBDIR}
  libslax extdir:   ${SLAX_EXTDIR}

  warnings:         ${YANGC_WARNINGS:-no}
  debug:            ${YANGC_DEBUG:-no}
  readline:         ${HAVE_READLINE:-no}
  libedit:          ${HAVE_LIBEDIT:-no}
  printf-like:      ${HAVE_PRINTFLIKE:-no}
])
