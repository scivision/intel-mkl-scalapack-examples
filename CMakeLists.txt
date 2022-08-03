cmake_minimum_required(VERSION 3.20.2...3.24)

project(ScalapackExamples LANGUAGES Fortran)

if(NOT DEFINED ENV{MKLROOT})
  message(FATAL_ERROR "MKL not detected via MKLROOT environment variable")
endif()

cmake_path(CONVERT "$ENV{MKLROOT}" TO_CMAKE_PATH_LIST MKLROOT NORMALIZE)

message(STATUS "MKLROOT: ${MKLROOT}")

include(ExternalProject)

find_file(archive
NAMES examples_cluster_f.tgz examples_cluster_f.zip
HINTS $ENV{MKLROOT}
PATH_SUFFIXES examples
NO_DEFAULT_PATH
REQUIRED
)

ExternalProject_Add(examples
URL ${archive}
CONFIGURE_HANDLED_BY_BUILD true
CMAKE_ARGS -DCMAKE_MODULE_PATH:PATH=$ENV{MKLROOT}/examples/cmake
INSTALL_COMMAND ""
TEST_BEFORE_INSTALL true
)
