include(GNUInstallDirs)

set(PACKAGE mecab-ipadic)
set(VERSION 2.7.0-20070801)

set(MECAB_DICT_INDEX "${CMAKE_CURRENT_BINARY_DIR}/../mecab/mecab-dict-index")
set(MECAB_IPADIC_CHARSET "utf-8")  # (euc-jp/sjis/utf-8)
set(MECAB_IPADIC_DIR "${CMAKE_INSTALL_FULL_LIBDIR}/mecab/dic/ipadic")

set(MECAB_IPADIC_BYPRODUCTS
  "${CMAKE_CURRENT_BINARY_DIR}/matrix.bin"
  "${CMAKE_CURRENT_BINARY_DIR}/char.bin"
  "${CMAKE_CURRENT_BINARY_DIR}/sys.dic"
  "${CMAKE_CURRENT_BINARY_DIR}/unk.dic"
)

add_custom_target(
  mecab-ipadic ALL
  COMMAND ${MECAB_DICT_INDEX}${CMAKE_EXECUTABLE_SUFFIX} -d ${CMAKE_CURRENT_SOURCE_DIR} -o ${CMAKE_CURRENT_BINARY_DIR} -f EUC-JP -t ${MECAB_IPADIC_CHARSET}
  DEPENDS ${MECAB_DICT_INDEX}
  BYPRODUCTS ${MECAB_IPADIC_BYPRODUCTS}
)

set(MECAB_IPADIC_DATA
  ${MECAB_IPADIC_BYPRODUCTS}
  "${CMAKE_CURRENT_SOURCE_DIR}/char.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/feature.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/left-id.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/matrix.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/pos-id.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/rewrite.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/right-id.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/unk.def"
  "${CMAKE_CURRENT_SOURCE_DIR}/dicrc"
)

install(FILES
  ${MECAB_IPADIC_DATA}
  DESTINATION "${MECAB_IPADIC_DIR}"
  )
