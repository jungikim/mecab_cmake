include(GNUInstallDirs)

set(MECAB_DICDIR "${CMAKE_INSTALL_FULL_LIBDIR}/mecab/dic/unidic_lite")

find_package(Python COMPONENTS Interpreter REQUIRED)

set(PYPKG unidic-lite==1.0.8)
set(TARGETDIR "${CMAKE_CURRENT_BINARY_DIR}/unidic-lite_1.0.8")

set(DIC_DATA
  "${TARGETDIR}/unidic_lite/dicdir/matrix.bin"
  "${TARGETDIR}/unidic_lite/dicdir/char.bin"
  "${TARGETDIR}/unidic_lite/dicdir/sys.dic"
  "${TARGETDIR}/unidic_lite/dicdir/unk.dic"
  "${TARGETDIR}/unidic_lite/dicdir/left-id.def"
  "${TARGETDIR}/unidic_lite/dicdir/rewrite.def"
  "${TARGETDIR}/unidic_lite/dicdir/right-id.def"
  "${TARGETDIR}/unidic_lite/dicdir/dicrc"
)

add_custom_target(
  mecab-unidic-lite ALL
  COMMAND ${Python_EXECUTABLE} -m pip install ${PYPKG} -t ${TARGETDIR} --upgrade
  BYPRODUCTS "${DIC_DATA}"
)

install(FILES
  ${DIC_DATA}
  DESTINATION "${MECAB_DICDIR}"
  )
