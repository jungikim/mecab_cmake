cmake_minimum_required(VERSION 3.1.0)
project(mecab)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif(NOT CMAKE_BUILD_TYPE)

add_subdirectory(mecab)
add_subdirectory(mecab-ipadic)
add_subdirectory(mecab-jumandic)
add_subdirectory(mecab-unidic-lite)
