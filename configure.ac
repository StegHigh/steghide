dnl Process this file with autoconf to produce a configure script.
AC_INIT
AC_CONFIG_SRCDIR([src/main.cc])
AC_CANONICAL_TARGET
AM_INIT_AUTOMAKE(steghide, 0.6.0)
AM_CONFIG_HEADER(config.h)

dnl checks for programs.
AC_PROG_CXX
AC_PROG_INSTALL
AC_PROG_AWK
AC_PROG_LN_S

dnl GNU gettext
AC_CHECK_FUNCS(strchr)
AM_GNU_GETTEXT
AM_CONDITIONAL(USE_INTLDIR, test "$nls_cv_use_gnu_gettext" = yes)

dnl check if debugging support is requested
AC_MSG_CHECKING([wether to enable debugging])
AC_ARG_ENABLE(debug,[  --enable-debug          enable debugging],
	if test "$enableval" = yes ;
	then
		AC_MSG_RESULT([yes])
		AC_DEFINE(DEBUG,1,[enable code used only for debugging])
		CXXFLAGS="-O2 -Wall -g"
	else
		AC_MSG_RESULT([no])
		CXXFLAGS="-O2 -Wall"
	fi
	,
	AC_MSG_RESULT([no])
	CXXFLAGS="-O2 -Wall"
)

dnl check if randomness should be disabled
AC_MSG_CHECKING([wether to disable randomness])
AC_ARG_ENABLE(randomness,[  --disable-randomness    disable randomness making every run exactly reproducible],
		if test "$enableval" = yes ;
		then
			AC_MSG_RESULT([no])
		else	
			AC_MSG_RESULT([yes])
			AC_DEFINE(NORANDOM,1,[disable randomness making every run exactly reproducible])
		fi
		,
		AC_MSG_RESULT([no])
)

dnl Checks for header files.
m4_warn([obsolete],
[The preprocessor macro `STDC_HEADERS' is obsolete.
  Except in unusual embedded environments, you can safely include all
  ISO C90 headers unconditionally.])dnl
# Autoupdate added the next two lines to ensure that your configure
# script's behavior did not change.  They are probably safe to remove.
AC_CHECK_INCLUDES_DEFAULT
AC_PROG_EGREP

AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS(limits.h termios.h)

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_TYPE_SIZE_T

dnl Check size of types
AC_CHECK_SIZEOF(unsigned long int)
AC_CHECK_SIZEOF(long int)
AC_CHECK_SIZEOF(unsigned int)
AC_CHECK_SIZEOF(int)
AC_CHECK_SIZEOF(unsigned short int)
AC_CHECK_SIZEOF(short int)
AC_CHECK_SIZEOF(unsigned char)
AC_CHECK_SIZEOF(signed char)

dnl searching for unsigned 32 bit integer
if test "$ac_cv_sizeof_unsigned_int" = 4;
then
	AC_DEFINE(TYPE_UWORD32,[unsigned int],[type of unsigned word with 32 bits])
else
if test "$ac_cv_sizeof_unsigned_long_int" = 4;
then
	AC_DEFINE(TYPE_UWORD32,[unsigned long int],[type of unsigned word with 32 bits])
else
	AC_MSG_ERROR([[There is no unsigned integer type with a size of 32 bits on your system. Cannot compile.]])
fi
fi	

dnl searching for signed 32 bit integer
if test "$ac_cv_sizeof_int" = 4;
then
	AC_DEFINE(TYPE_SWORD32,[int],[type of signed word with 32 bits])
else
if test "$ac_cv_sizeof_long_int" = 4;
then
	AC_DEFINE(TYPE_SWORD32,[long int],[type of signed word with 32 bits])
else
	AC_MSG_ERROR([[There is no signed integer type with a size of 32 bits on your system. Cannot compile.]])
fi
fi	

dnl searching for unsigned 16 bit integer
if test "$ac_cv_sizeof_unsigned_short_int" = 2;
then
	AC_DEFINE(TYPE_UWORD16,[unsigned short int],[type of unsigned word with 16 bits])
else
if test "$ac_cv_sizeof_unsigned_int" = 2;
then
	AC_DEFINE(TYPE_UWORD16,[unsigned int],[type of unsigned word with 16 bits])
else
	AC_MSG_ERROR([[There is no unsigned integer type with a size of 16 bits on your system. Cannot compile.]])
fi
fi	

dnl searching for signed 16 bit integer
if test "$ac_cv_sizeof_short_int" = 2;
then
	AC_DEFINE(TYPE_SWORD16,[short int],[type of signed word with 16 bits])
else
if test "$ac_cv_sizeof_int" = 2;
then
	AC_DEFINE(TYPE_SWORD16,[int],[type of signed word with 16 bits])
else
	AC_MSG_ERROR([[There is no signed integer type with a size of 16 bits on your system. Cannot compile.]])
fi
fi	

dnl searching for unsigned 8 bit character
if test "$ac_cv_sizeof_unsigned_char" = 1;
then
	AC_DEFINE(TYPE_BYTE,[unsigned char],[type of unsigned byte])
else
	AC_MSG_ERROR([[There is no unsigned character type with a size of 8 bits on your system. Cannot compile.]])
fi	

dnl searching for signed 8 bit character
if test "$ac_cv_sizeof_signed_char" = 1;
then
	AC_DEFINE(TYPE_SBYTE,[signed char],[type of signed byte])
else
	AC_MSG_ERROR([[There is no signed character type with a size of 8 bits on your system. Cannot compile.]])
fi	

dnl check for doxygen
AC_CHECK_PROG(ac_cv_prog_doxygen, "doxygen", yes, no)
AM_CONDITIONAL(HAVE_DOXYGEN, test "$ac_cv_prog_doxygen" = yes)

dnl check for perl
AC_CHECK_PROG(ac_cv_prog_perl, "perl", yes, no)
AM_CONDITIONAL(HAVE_PERL, test "$ac_cv_prog_perl" = yes)

dnl Check for random sources	
AC_CHECK_FILE(/dev/urandom, [AC_DEFINE(HAVE_DEV_URANDOM,1,/dev/urandom is available)])
AC_CHECK_FILE(/dev/random, [AC_DEFINE(HAVE_DEV_RANDOM,1,/dev/random is available)])

dnl Checks for library functions.
AC_FUNC_VPRINTF
AC_CHECK_FUNCS(tcgetattr tcsetattr strtoul)

dnl system-specific settings
case "${target}" in
	*-cygwin*)
	AC_DEFINE(WIN32,1,target system is win32)
	;;
esac

dnl Check for zlib
AC_CHECK_LIB(z, zlibVersion)
AC_CHECK_HEADER(zlib.h)

dnl Check for libmhash
AC_CHECK_LIB(mhash, mhash_init)
AC_CHECK_HEADER(mhash.h)

dnl Check for libmcrypt
AC_CHECK_LIB(mcrypt, mcrypt_generic, , ,)
AC_CHECK_HEADER(mcrypt.h)

dnl Check for libjpeg
AC_CHECK_LIB(jpeg, jpeg_read_coefficients)
AC_CHECK_HEADER(jpeglib.h)

dnl evaluate library tests
if test "$ac_cv_header_jpeglib_h" = no || test "$ac_cv_lib_jpeg_jpeg_read_coefficients" = no ;
then
	echo "**********";
	echo "libjpeg could not be found on your system. Steghide will be compiled without";
	echo "support for jpeg files. You will not be able to read or write jpeg files!";
	echo "libjpeg can be downloaded from http://www.ijg.org/";
	echo "**********";
else
	AC_DEFINE(USE_LIBJPEG,1,[use the libjpeg library (header and lib are present)])
fi

if test "$ac_cv_header_mcrypt_h" = no || test "$ac_cv_lib_mcrypt_mcrypt_module_open" = no ;
then
	echo "**********";
	echo "libmcrypt could not be found on your system. Steghide will be compiled without";
	echo "support for encryption. You will not be able to extract encrypted data!";
	echo "libmcrypt can be downloaded from http://mcrypt.sourceforge.net/";
	echo "**********";
else
	AC_DEFINE(USE_LIBMCRYPT,1,[use the libmcrypt library (header and lib are present)])
fi	

if test "$ac_cv_header_zlib_h" = no || test "$ac_cv_lib_z_zlibVersion" = no ;
then
	echo "**********";
	echo "zlib could not be found on your system. Steghide will be compiled without";
	echo "the ability to compress and uncompress embedded data!";
	echo "zlib can be downloaded from http://www.gzip.org/zlib/";
	echo "**********";
else
	AC_DEFINE(USE_ZLIB,1,[use the z compression library (header and lib are present)])
fi

if test "$ac_cv_header_mhash_h" = no || test "$ac_cv_lib_mhash_mhash_init" = no ;
then
	echo "**********";
	echo "libmhash could not be found on your system but is needed to compile steghide.";
	echo "libmhash can be downloaded from http://mhash.sourceforge.net/.";
	echo "**********";
	AC_MSG_ERROR([[libmhash not found]])
fi

dnl create Makefiles
AC_CONFIG_FILES([Makefile steghide.spec steghide.doxygen doc/Makefile po/Makefile.in src/Makefile tests/Makefile tests/data/Makefile m4/Makefile intl/Makefile])
AC_OUTPUT
