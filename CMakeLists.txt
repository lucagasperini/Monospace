# Copyright (C) 2021-2023 Luca Gasperini <luca.gasperini@xsoftware.it>
#
# This file is part of Monospace.
#
# Monospace is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 2 of the License, or
# (at your option) any later version.
#
# Monospace is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with Monospace.  If not, see <http://www.gnu.org/licenses/>.


project(master_tracker)

cmake_minimum_required(VERSION 3.9)

include(GNUInstallDirs)


# Tell CMake to compile with C++20
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Search on the system Doxygen
find_package(Doxygen)

if (DOXYGEN_FOUND)

    set(DOXYGEN_EXCLUDE_PATTERNS "*/build/*")

    doxygen_add_docs(
        doc_master_tracker
        WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}

        COMMENT "Generating API documentation with Doxygen"
    )
else (DOXYGEN_FOUND)
  message("Doxygen need to be installed to generate the doxygen documentation")
endif (DOXYGEN_FOUND)

if (CMAKE_BUILD_TYPE STREQUAL "Debug")
    enable_testing()
    add_definitions(-DMTK_DEBUG=1)

    # TODO: For windows?
#if (UNIX)
#    set(CAMOMILLA_PYTHON "/usr/bin/python3 ${CMAKE_SOURCE_DIR}/src/scripts/camomilla/camomilla")

#    set(CMAKE_CXX_COMPILE_OBJECT "${CMAKE_CXX_COMPILE_OBJECT} 2>&1 | ${CAMOMILLA_PYTHON} -r -d1")
#    set(CMAKE_C_COMPILE_OBJECT "${CMAKE_CXX_COMPILE_OBJECT} 2>&1 | ${CAMOMILLA_PYTHON} -r -d1")
#endif()
endif()

add_compile_options(-Wall) # Enable all compiler warning
add_compile_options(-pipe) # Use pipes rather than temporary files for communication between the various stages of compilation.


include_directories(
    ${CMAKE_SOURCE_DIR}/include
)


add_subdirectory(src)
add_subdirectory(app)
add_subdirectory(tests)