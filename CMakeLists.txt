# (C) Copyright 2017-2021 UCAR
#
# This software is licensed under the terms of the Apache Licence Version 2.0
# which can be obtained at http://www.apache.org/licenses/LICENSE-2.0.

#
# IODA bundle
#

project( ioda-bundle C CXX Fortran )

cmake_minimum_required( VERSION 3.3.2 FATAL_ERROR )

list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include( ecbuild_bundle )

set( ENABLE_MPI ON CACHE BOOL "Compile with MPI" )

ecbuild_bundle_initialize()

ecbuild_requires_macro_version( 2.7 )

#ecbuild_add_option( FEATURE OMP
#                    DEFAULT ON
#                    DESCRIPTION "Use OpenMP" )

ecbuild_bundle( PROJECT jedicmake GIT "https://github.com/JCSDA-internal/jedi-cmake.git" BRANCH develop UPDATE )

find_package(eckit)
find_package(fckit)
find_package(atlas)

if(NOT eckit_FOUND)
  ecbuild_bundle( PROJECT eckit GIT "https://github.com/JCSDA-internal/eckit.git" BRANCH release-stable UPDATE )
endif()

if(NOT fckit_FOUND)
  ecbuild_bundle( PROJECT fckit GIT "https://github.com/JCSDA-internal/fckit.git" BRANCH release-stable UPDATE )
endif()

if(NOT atlas_FOUND)
  ecbuild_bundle( PROJECT atlas GIT "https://github.com/JCSDA-internal/atlas.git" BRANCH release-stable  UPDATE )
endif()

ecbuild_bundle( PROJECT oops GIT "https://github.com/JCSDA-internal/oops.git" BRANCH develop UPDATE )
ecbuild_bundle( PROJECT ioda GIT "https://github.com/JCSDA-internal/ioda.git" BRANCH develop UPDATE )

# Build IODA converters
option(BUILD_IODA_CONVERTERS "Build IODA Converters" OFF)
if(BUILD_IODA_CONVERTERS)
  ecbuild_bundle( PROJECT iodaconv GIT "https://github.com/JCSDA-internal/ioda-converters.git" BRANCH develop UPDATE )
endif()

# Build Doxygen documentation
option(BUILD_IODA_BUNDLE_DOC "Build documentation" OFF)
if(BUILD_IODA_BUNDLE_DOC)
   add_subdirectory( Documentation )
endif(BUILD_IODA_BUNDLE_DOC)

ecbuild_bundle_finalize()

include(cmake/cdash-integration.cmake)
include(CTest)
