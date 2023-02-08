# ------------------------------------------------------------------------------------------------ #
#                                This file is part of CosmoScout VR                                #
# ------------------------------------------------------------------------------------------------ #

# SPDX-FileCopyrightText: German Aerospace Center (DLR) <cosmoscout@dlr.de>
# SPDX-License-Identifier: MIT

option(CSP_VAMEX "Enable compilation of this plugin" ON)

if (NOT CSP_VAMEX)
  return()
endif()

# build plugin -------------------------------------------------------------------------------------

file(GLOB SOURCE_FILES src/*.cpp src/internal/*.cpp)

# Resource files and header files are only added in order to make them available in your IDE.
file(GLOB HEADER_FILES src/*.hpp)
file(GLOB_RECURSE RESOURCE_FILES gui/*)

add_library(csp-vamex SHARED
  ${SOURCE_FILES}
  ${HEADER_FILES}
  ${RESOURCE_FILES}
)

target_link_libraries(csp-vamex
  PUBLIC
    cs-core
    cs-utils
    civetweb::civetweb
    civetweb::civetwebcpp
)

# Add this Plugin to a "plugins" folder in your IDE.
set_property(TARGET csp-vamex PROPERTY FOLDER "plugins")

# We mark all resource files as "header" in order to make sure that no one tries to compile them.
set_source_files_properties(${RESOURCE_FILES} PROPERTIES HEADER_FILE_ONLY TRUE)

# Make directory structure available in your IDE.
source_group(TREE "${CMAKE_CURRENT_SOURCE_DIR}" FILES
  ${SOURCE_FILES} ${HEADER_FILES} ${RESOURCE_FILES}
)

target_include_directories(csp-vamex PUBLIC
    ${CMAKE_BINARY_DIR}/plugins/csp-vamex
)

# install plugin -----------------------------------------------------------------------------------

install(TARGETS csp-vamex DESTINATION "share/plugins")
install(DIRECTORY "gui"   DESTINATION "share/resources")

# export header ------------------------------------------------------------------------------------

generate_export_header(csp-vamex
    EXPORT_FILE_NAME csp_vamex_export.hpp
)
