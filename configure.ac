AC_PREREQ(2.52)

AC_INIT([apertium-fra-eng], [0.1.0], [unknown@users.sourceforge.net])
AM_INIT_AUTOMAKE([apertium-fra-eng], [0.1.0], no-define)
AC_PROG_LN_S
AC_PROG_AWK

AC_DEFUN([PKG_CHECK_APERTIUM], [
  APERTIUM_VER=0
  PKG_CHECK_MODULES(APERTIUM, apertium-3.2 >= 3.2.0,
  [
      APERTIUM_VER=32
  ],
  [
      PKG_CHECK_MODULES(APERTIUM, apertium-3.1 >= 3.1.0,
      [
          APERTIUM_VER=31
      ],
      [
          PKG_CHECK_MODULES(APERTIUM, apertium-3.0 >= 3.0.0,
          [
              APERTIUM_VER=30
          ])
      ])
  ])
  AC_SUBST(APERTIUM_VER)
])
                          
PKG_CHECK_APERTIUM

AC_PATH_PROG(HFSTLEXC, hfst-lexc, no)
if test x$ac_cv_path_HFSTLEXC = x
then
  AC_MSG_ERROR([You don't have hfst-lexc installed.])
fi
if test x$ac_cv_path_HFSTLEXC = xno
then
  AC_MSG_ERROR([You don't have hfst-lexc installed.])
fi

AC_PATH_PROG(HFSTTWOLC, hfst-twolc, no)
if test x$ac_cv_path_HFSTTWOLC = x
then
  AC_MSG_ERROR([You don't have hfst-twolc installed.])
fi
if test x$ac_cv_path_HFSTTWOLC = xno
then
  AC_MSG_ERROR([You don't have hfst-twolc installed.])
fi

AC_PATH_PROG(CGCOMP, cg-comp, no)
if test x$ac_cv_path_CGCOMP = x
then
  AC_MSG_ERROR([You don't have cg-comp installed.])
fi
if test x$ac_cv_path_CGCOMP = xno
then
  AC_MSG_ERROR([You don't have cg-comp installed.])
fi

AC_PATH_PROG(CGPROC, cg-proc, no)
if test x$ac_cv_path_CGPROC = x
then
  AC_MSG_ERROR([You don't have cg-proc installed.])
fi
if test x$ac_cv_path_CGPROC = xno
then
  AC_MSG_ERROR([You don't have cg-proc installed.])
fi

AC_PATH_PROG(LRXCOMP, lrx-comp, no)
if test x$ac_cv_path_LRXCOMP = x
then
  AC_MSG_ERROR([You don't have lrx-comp installed.])
fi
if test x$ac_cv_path_LRXCOMP = xno
then
  AC_MSG_ERROR([You don't have lrx-comp installed.])
fi

AC_PATH_PROG(LRXPROC, lrx-proc, no)
if test x$ac_cv_path_LRXPROC = x
then
  AC_MSG_ERROR([You don't have lrx-proc installed.])
fi
if test x$ac_cv_path_LRXPROC = xno
then
  AC_MSG_ERROR([You don't have lrx-proc installed.])
fi


AC_OUTPUT([Makefile])

