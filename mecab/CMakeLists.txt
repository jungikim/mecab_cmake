# workaround for windows config-based directories
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")
SET(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")
SET(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")
SET(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE "${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")

include(GNUInstallDirs)
include(FindIconv)
include(CheckCXXSourceCompiles)
include(CheckIncludeFile)
include(CheckFunctionExists)

set(PACKAGE mecab)
set(VERSION 0.996)
set(MECAB_DIC_VERSION "102")
set(MECAB_USE_UTF8_ONLY "-DMECAB_USE_UTF8_ONLY")
set(MECAB_DEFAULT_RC ${CMAKE_INSTALL_SYSCONFDIR}/mecabrc)
set(MECAB_LTVERSION "2:0:0")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -Wall")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wall")

add_compile_options(
	-DPACKAGE="${PACKAGE}"
	-DVERSION="${VERSION}"
	-DDIC_VERSION=${MECAB_DIC_VERSION}
  ${MECAB_USE_UTF8_ONLY}
  -DMECAB_DEFAULT_RC="${MECAB_DEFAULT_RC}"
)
set(CMAKE_MODULE_LINKER_FLAGS -no-undefined -version-info ${MECAB_LTVERSION})

option(BUILD_SHARED_LIBS "Build shared libraries" ON)


message(STATUS "Iconv_FOUND: \"${Iconv_FOUND}\"")
message(STATUS "Iconv_INCLUDE_DIR: \"${Iconv_INCLUDE_DIR}\"")
message(STATUS "Iconv_LIBRARIES: \"${Iconv_LIBRARIES}\"")
if(Iconv_FOUND)
  check_cxx_source_compiles("
  #include <iconv.h>
  int main(){
    iconv_t conv = 0;
    const char* in = 0;
    size_t ilen = 0;
    char* out = 0;
    size_t olen = 0;
    iconv(conv, &in, &ilen, &out, &olen);
    return 0;
  }
" ICONV_SECOND_ARGUMENT_IS_CONST)
  if(ICONV_SECOND_ARGUMENT_IS_CONST)
    set(Iconv_CONST "const")
  endif(ICONV_SECOND_ARGUMENT_IS_CONST)
endif(Iconv_FOUND)
message(STATUS "Iconv_CONST: \"${Iconv_CONST}\"")

check_include_file("config.h" HAVE_CONFIG_H)
check_include_file("windows.h" HAVE_WINDOWS_H)
check_include_file("dlfcn.h" HAVE_DLFCN_H)
check_include_file("dirent.h" HAVE_DIRENT_H)
check_include_file("fcntl.h" HAVE_FCNTL_H)
check_include_file("iconv.h" HAVE_ICONV)
check_include_file("inttypes.h" HAVE_INTTYPES_H)
check_include_file("memory.h" HAVE_MEMORY_H)
check_include_file("pthread.h" HAVE_PTHREAD_H)
check_cxx_source_compiles("
  int main(){
    __thread int a = 0;
    a = 10;
    return 0;
  }
" HAVE_TLS_KEYWORD)
check_include_file("stdint.h" HAVE_STDINT_H)
check_include_file("stdlib.h" HAVE_STDLIB_H)
check_include_file("strings.h" HAVE_STRINGS_H)
check_include_file("string.h" HAVE_STRING_H)
check_include_file("sys/stat.h" HAVE_SYS_STAT_H)
check_include_file("sys/types.h" HAVE_SYS_TYPES_H)
check_include_file("sys/mman.h" HAVE_SYS_MMAN_H)
check_include_file("unistd.h" HAVE_UNISTD_H)
check_include_file("zlib.h" HAVE_ZLIB_H)

check_function_exists("mmap" HAVE_MMAP)
check_function_exists("getenv" HAVE_GETENV)
set(HAVE_UNSIGNED_LONG_LONG_INT 1)
check_cxx_source_compiles("
  #include <sched.h>
  int main(){
    int a = 10;
    __sync_fetch_and_add(&a, 10);
    __sync_val_compare_and_swap(&a, 0, 10);
    sched_yield();
    return 0;
  }
" HAVE_GCC_ATOMIC_OPS)
check_cxx_source_compiles("
  #include <libkern/OSAtomic.h>
  int main(){
    int a = 10;
    OSAtomicAdd32(10, &a);
    OSAtomicCompareAndSwapInt(10, 0, &a);
    return 0;
  }
" HAVE_GCC_ATOMIC_OPS)
if(HAVE_CONFIG_H)
  add_compile_options(-DHAVE_CONFIG_H)
endif()
if(HAVE_WINDOWS_H)
  add_compile_options(-DHAVE_WINDOWS_H)
endif()
if(HAVE_DLFCN_H)
  add_compile_options(-DHAVE_DLFCN_H)
endif()
if(HAVE_DIRENT_H)
  add_compile_options(-DHAVE_DIRENT_H)
endif()
if(HAVE_FCNTL_H)
  add_compile_options(-DHAVE_FCNTL_H)
endif()
if(HAVE_ICONV)
  add_compile_options(-DHAVE_ICONV -DICONV_CONST=${Iconv_CONST})
endif()
if(HAVE_INTTYPES_H)
  add_compile_options(-DHAVE_INTTYPES_H)
endif()
if(HAVE_MEMORY_H)
  add_compile_options(-DHAVE_MEMORY_H)
endif()
if(HAVE_PTHREAD_H)
  add_compile_options(-DHAVE_PTHREAD_H)
  find_package(Threads REQUIRED)
  if(THREADS_HAVE_PTHREAD_ARG)
    add_compile_options(-pthread)
  endif()
endif()
if(HAVE_TLS_KEYWORD)
  add_compile_options(-DHAVE_TLS_KEYWORD)
endif()
if(HAVE_STDINT_H)
  add_compile_options(-DHAVE_STDINT_H)
endif()
if(HAVE_STDLIB_H)
  add_compile_options(-DHAVE_STDLIB_H)
endif()
if(HAVE_STRINGS_H)
  add_compile_options(-DHAVE_STRINGS_H)
endif()
if(HAVE_STRING_H)
  add_compile_options(-DHAVE_STRING_H)
endif()
if(HAVE_SYS_STAT_H)
  add_compile_options(-DHAVE_SYS_STAT_H)
endif()
if(HAVE_SYS_TYPES_H)
  add_compile_options(-DHAVE_SYS_TYPES_H)
endif()
if(HAVE_SYS_MMAN_H)
  add_compile_options(-DHAVE_SYS_MMAN_H)
endif()
if(HAVE_UNISTD_H)
  add_compile_options(-DHAVE_UNISTD_H)
endif()
if(HAVE_ZLIB_H)
  add_compile_options(-DHAVE_ZLIB_H)
endif()

if(HAVE_GETENV)
  add_compile_options(-DHAVE_GETENV)
endif()
if(HAVE_UNSIGNED_LONG_LONG_INT)
  add_compile_options(-DHAVE_UNSIGNED_LONG_LONG_INT)
endif()
if(HAVE_MMAP)
  add_compile_options(-DHAVE_MMAP)
endif()
if(HAVE_GCC_ATOMIC_OPS)
  add_compile_options(-DHAVE_GCC_ATOMIC_OPS)
endif()
if(HAVE_OSX_ATOMIC_OPS)
  add_compile_options(-DHAVE_OSX_ATOMIC_OPS)
endif()

set(MECAB_LIBS "-lstdc++")
if(Iconv_LIBRARIES)
  set(MECAB_LIBS "${MECAB_LIBS} ${Iconv_LIBRARIES}")
endif()
if(CMAKE_THREAD_LIBS_INIT)
 set(MECAB_LIBS "${MECAB_LIBS} ${CMAKE_THREAD_LIBS_INIT}")
endif()

message(STATUS "MECAB_LIBS: \"${MECAB_LIBS}\"")

set(CXX ${CMAKE_CXX_COMPILER})
set(CC ${CMAKE_C_COMPILER})
set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix ${CMAKE_INSTALL_PREFIX})
set(libdir ${CMAKE_INSTALL_LIBDIR})
set(includedir ${CMAKE_INSTALL_INCLUDEDIR})
set(sysconfdir ${CMAKE_INSTALL_SYSCONFDIR})

configure_file(mecab-config.in mecab-config @ONLY)
configure_file(mecabrc.in mecabrc @ONLY)

set(MECAB_SOURCES
  src/viterbi.cpp
  src/tagger.cpp
  src/utils.cpp
  src/utils.h
  src/eval.cpp
  src/iconv_utils.cpp
  src/iconv_utils.h
  src/dictionary_rewriter.h
  src/dictionary_rewriter.cpp
  src/dictionary_generator.cpp
  src/dictionary_compiler.cpp
  src/context_id.h
  src/context_id.cpp
  src/winmain.h
  src/thread.h
  src/connector.cpp
  src/nbest_generator.h
  src/nbest_generator.cpp
  src/connector.h
  src/writer.h
  src/writer.cpp
  src/mmap.h
  src/ucs.h 
  src/string_buffer.h
  src/string_buffer.cpp
  src/tokenizer.h
  src/stream_wrapper.h
  src/common.h
  src/darts.h
  src/char_property.h
  src/ucstable.h
  src/freelist.h
  src/viterbi.h
  src/param.cpp
  src/tokenizer.cpp
  src/ucstable.h
  src/char_property.cpp
  src/dictionary.h
  src/scoped_ptr.h
  src/param.h
  src/mecab.h
  src/dictionary.cpp
  src/feature_index.cpp
  src/feature_index.h
  src/lbfgs.cpp
  src/lbfgs.h
  src/learner_tagger.cpp
  src/learner_tagger.h
  src/learner.cpp
  src/learner_node.h
  src/libmecab.cpp
  )

if(COMMAND create_library)
  create_library(mecab SHARED ${MECAB_SOURCES})
else()
  add_library(mecab ${MECAB_SOURCES})
endif()
set_target_properties(mecab PROPERTIES VERSION ${VERSION})
target_include_directories(mecab PUBLIC ${CMAKE_SOURCE_DIR})
target_link_libraries(mecab ${MECAB_LIBS})

add_executable(mecabbin src/mecab.cpp)
if(NOT CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
  set_property(TARGET mecabbin PROPERTY OUTPUT_NAME mecab)
endif()
add_executable(mecab-dict-index src/mecab-dict-index.cpp)
add_executable(mecab-dict-gen src/mecab-dict-gen.cpp)
add_executable(mecab-cost-train src/mecab-cost-train.cpp)
add_executable(mecab-system-eval src/mecab-system-eval.cpp)
add_executable(mecab-test-gen src/mecab-test-gen.cpp)

target_link_libraries(mecabbin mecab ${MECAB_LIBS})
target_link_libraries(mecab-dict-index mecab ${MECAB_LIBS})
target_link_libraries(mecab-dict-gen mecab ${MECAB_LIBS})
target_link_libraries(mecab-cost-train mecab ${MECAB_LIBS})
target_link_libraries(mecab-system-eval mecab ${MECAB_LIBS})
target_link_libraries(mecab-test-gen mecab ${MECAB_LIBS})


install(
  TARGETS mecab mecabbin
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
  )
install(FILES
  src/mecab.h
  DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}"
  )
install(
  TARGETS mecab-dict-index mecab-dict-gen mecab-cost-train mecab-system-eval mecab-test-gen
  DESTINATION ${CMAKE_INSTALL_LIBEXECDIR}
  )
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/mecabrc
  DESTINATION ${CMAKE_INSTALL_SYSCONFDIR}
  )
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/mecab-config
  DESTINATION ${CMAKE_INSTALL_BINDIR}
  PERMISSIONS OWNER_EXECUTE OWNER_READ GROUP_EXECUTE GROUP_READ WORLD_EXECUTE WORLD_READ
  )
