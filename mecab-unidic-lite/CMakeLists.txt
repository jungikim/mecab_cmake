include(GNUInstallDirs)

set(MECAB_DICDIR "${CMAKE_INSTALL_FULL_LIBDIR}/mecab/dic/unidic_lite")

find_package(Python COMPONENTS Interpreter REQUIRED)

set(PYPKG unidic-lite==1.0.8)
set(TARGETDIR "${CMAKE_CURRENT_BINARY_DIR}/unidic-lite_1.0.8")

execute_process(COMMAND ${Python_EXECUTABLE} -m pip install ${PYPKG} -t ${TARGETDIR})

file(GLOB DIC_DATA
     "${TARGETDIR}/unidic_lite/dicdir/matrix.bin"
     "${TARGETDIR}/unidic_lite/dicdir/char.bin"
     "${TARGETDIR}/unidic_lite/dicdir/sys.dic"
     "${TARGETDIR}/unidic_lite/dicdir/unk.dic"
     "${TARGETDIR}/unidic_lite/dicdir/*.def"
     "${TARGETDIR}/unidic_lite/dicdir/dicrc"
)

# message(STATUS "CMAKE_INSTALL_LIBDIR: \"${CMAKE_INSTALL_FULL_LIBDIR}\"")
# message(STATUS "MECAB_DICDIR: \"${MECAB_DICDIR}\"")
# message(STATUS "CMAKE_CURRENT_BINARY_DIR: \"${CMAKE_CURRENT_BINARY_DIR}\"")
# message(STATUS "TARGETDIR: \"${TARGETDIR}\"")
# message(STATUS "DIC_DATA: \"${DIC_DATA}\"")

install(FILES
  ${DIC_DATA}
  DESTINATION "${MECAB_DICDIR}"
  )
