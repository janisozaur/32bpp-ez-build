AC_INIT([32bpp_ez-build],[0.1])
AC_CONFIG_SRCDIR([build_nml.in])
AC_PROG_MAKE_SET
AC_PROG_MKDIR_P
[BASH="/bin/bash"]
AC_PATH_PROG([BASH],[bash],[not found])
AS_IF([test "x$BASH" = "xnot found"],
  [AC_MSG_ERROR([Cannot find suitable bash executable. You need to install a recent Bourne again shell (bash) version in your
   PATH, and run configure again.])])
AC_PROG_SED
AC_PROG_AWK
AC_PROG_GREP
AC_PROG_EGREP
AC_ARG_WITH([nmlc],
  [AS_HELP_STRING([--with-nmlc], [Path to your nmlc compiler executable (default: search in PATH)])],
  [],
  [[with_nmlc=path]])
  
AS_IF([test "x$with_nmlc" != xpath],
  [[NMLC="$with_nmlc"]],
  [AC_PATH_PROG([NMLC],[nmlc],[not found])])
AS_IF([test "x$NMLC" = "xnot found"],
  [AC_MSG_ERROR([Cannot find suitable nmlc executable. Please download and install a recent (>= 0.3.0) version from http://bundles.openttdcoop.org/nml/releases. You can set the explicit path to the nmlc executable using the --with-nmlc=<path-to-nmlc> configuration option.])])
AC_MSG_CHECKING([whether "$NMLC" can embed 32bpp graphics])
AS_IF(
[AS_TMPDIR([32bpp_ez-build.conf])
[echo '
grf {
    grfid: "DUMM";
    name: string(GRF_NAME);
    desc: string(GRF_DESC);
    version: 1;
    min_compatible_version: 0;
}

replace spr_dummy(1, "'"$srcdir"'/empty.pcx") {
    [0, 0, 1, 1, 0, 0]
}
alternative_sprites(spr_dummy, ZOOM_LEVEL_IN_4X, BIT_DEPTH_32BPP) {
    [0, 0, "'"$srcdir"'/empty.png", []]
}
' > "$tmp"/dummy.nml && "$NMLC" "$tmp"/dummy.nml --lang-dir="$srcdir"/lang --grf="$tmp"/dummy.grf 2>&]AS_MESSAGE_LOG_FD],
  [AC_MSG_RESULT([yes])
   [rm -rf parsetab.py "$tmp"]],
  [AC_MSG_RESULT([no])
   [(echo Failed nml program was:
    cat "$tmp"/dummy.nml) >&]AS_MESSAGE_LOG_FD
   [(echo Failed nmlc invocation was:
    echo "$NMLC" "$tmp"/dummy.nml --lang-dir="$srcdir"/lang --grf="$tmp"/dummy.grf ) >&]AS_MESSAGE_LOG_FD
   [rm -rf parsetab.py "$tmp"]
   AC_MSG_FAILURE([["$NMLC" cannot compile grf files with 32bpp graphics. Please download and install a recent (>= 0.3.0) version from http://bundles.openttdcoop.org/nml/releases. You can set the explicit path to the nmlc executable using the --with-nmlc=<path-to-nmlc> configuration option.]])
  ])

AC_ARG_WITH([pngcodec],
  [AS_HELP_STRING([--with-pngcodec], [Path to your pngcodec executable (default: search in PATH)])],
  [],
  [[with_pngcodec=path]])

AS_IF([test "x$with_pngcodec" != xpath],
  [[PNGCODEC="$with_pngcodec"]],
  [AC_PATH_PROG([PNGCODEC],[pngcodec],[not found])])
AS_IF([test "x$PNGCODEC" = "xnot found"],
  [AC_MSG_ERROR([Cannot find suitable pngcodec executable. Please download and install a recent version from http://www.openttd.org/en/download-pngcodec. You can set the explicit path to the pngcodec executable using the --with-pngcodec=<path-to-pngcodec> configuration option.])])


AC_CONFIG_FILES([Makefile
                 base/Makefile
                 arctic/Makefile
		 tropical/Makefile
		 extra/Makefile])
AC_CONFIG_FILES([build_nml], [chmod +x build_nml])
AC_CONFIG_FILES([extra/build_nml.extra], [chmod +x extra/build_nml.extra])

AC_OUTPUT


