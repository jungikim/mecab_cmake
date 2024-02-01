include(GNUInstallDirs)

set(MECAB_UNIDIC_DIR "${CMAKE_INSTALL_FULL_LIBDIR}/mecab/dic/unidic_lite")

find_package(Python COMPONENTS Interpreter REQUIRED)

set(MECAB_UNIDIC_PYPKG unidic-lite==1.0.8)
set(MECAB_UNIDIC_TARGETDIR "${CMAKE_CURRENT_BINARY_DIR}/unidic-lite_1.0.8")

set(MECAB_UNIDIC_DATA
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/matrix.bin"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/char.bin"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/sys.dic"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/unk.dic"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/left-id.def"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/rewrite.def"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/right-id.def"
  "${MECAB_UNIDIC_TARGETDIR}/unidic_lite/dicdir/dicrc"
)

add_custom_target(
  mecab-unidic-lite ALL
  COMMAND ${Python_EXECUTABLE} -m pip install ${MECAB_UNIDIC_PYPKG} -t ${MECAB_UNIDIC_TARGETDIR} --upgrade
  BYPRODUCTS "${MECAB_UNIDIC_DATA}"
)

install(FILES
  ${MECAB_UNIDIC_DATA}
  DESTINATION "${MECAB_UNIDIC_DIR}"
  )
