cmake_minimum_required(VERSION 3.30)

project(lwgrp
VERSION 1.0.6
DESCRIPTION "Group MPI Processes"
HOMEPAGE_URL "https://github.com/llnl/lwgrp"
LANGUAGES C
)

include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

option(BUILD_SHARED_LIBS "Build LWGRP with shared libraries" ON)
option(USE_MPIANYSOURCE "Build with MPIANYSOURCE enabled" OFF)
option(LWGRP_ENABLE_TESTS "Build LWGRP tests" OFF)

find_package(MPI COMPONENTS C REQUIRED)

if(LWGRP_ENABLE_TESTS)
  enable_testing()
endif()

add_subdirectory(src)

install(TARGETS lwgrp
EXPORT lwgrp_targets
FILE_SET HEADERS DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
set(LWGRP_INSTALL_CMAKEDIR ${CMAKE_INSTALL_LIBDIR}/cmake/lwgrp)
install(EXPORT lwgrp_targets
    FILE lwgrpTargets.cmake
    NAMESPACE lwgrp::
    DESTINATION ${LWGRP_INSTALL_CMAKEDIR}
)
configure_package_config_file(cmake/lwgrpConfig.cmake.in ${CMAKE_CURRENT_BINARY_DIR}/lwgrpConfig.cmake
    INSTALL_DESTINATION ${LWGRP_INSTALL_CMAKEDIR}
)
write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/lwgrpConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/lwgrpConfig.cmake
              ${CMAKE_CURRENT_BINARY_DIR}/lwgrpConfigVersion.cmake
        DESTINATION ${LWGRP_INSTALL_CMAKEDIR}
)

# pkg-config file, using the same variables autoconf substitutes into liblwgrp.pc.in
set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix "\${prefix}")
set(libdir "\${exec_prefix}/${CMAKE_INSTALL_LIBDIR}")
set(includedir "\${prefix}/${CMAKE_INSTALL_INCLUDEDIR}")
set(PACKAGE_NAME ${PROJECT_DESCRIPTION})
set(PACKAGE_VERSION ${PROJECT_VERSION})
set(PACKAGE_URL ${PROJECT_HOMEPAGE_URL})
configure_file(liblwgrp.pc.in ${CMAKE_CURRENT_BINARY_DIR}/liblwgrp.pc @ONLY)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/liblwgrp.pc
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/pkgconfig
)

install(FILES README LICENSE.TXT
        DESTINATION ${CMAKE_INSTALL_DATADIR}/lwgrp
)


