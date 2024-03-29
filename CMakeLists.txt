cmake_minimum_required(VERSION 3.13)

message(STATUS "We have moved from M$ GitHub to https://codeberg.org/implode-compression-impls/pklib , read why on https://codeberg.org/KOLANICH/Fuck-GuanTEEnomo . ")

set(CMAKE_USE_RELATIVE_PATHS TRUE)
project("pklib")
set("PROJECT_DESCRIPTION" "Free open source PKWare Data Compression Library (DCL) compressor implementation")
set("PROJECT_HOMEPAGE_URL" "https://github.com/ladislav-zezula/StormLib/tree/master/src/pklib")
set(CPACK_PACKAGE_VENDOR "Ladislav Zezula")

set(CPACK_PACKAGE_VERSION_MAJOR "9")
set(CPACK_PACKAGE_VERSION_MINOR "23")
set(CPACK_PACKAGE_VERSION_PATCH "0")
set(CPACK_PACKAGE_VERSION "${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH}")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake" "${CMAKE_SOURCE_DIR}/cmake/Hardening")

set(OUR_CMAKE_MODULES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(OUR_CMAKE_3PARTY_MODULES_DIR "${OUR_CMAKE_MODULES_DIR}/thirdParty")
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${OUR_CMAKE_MODULES_DIR}" "${OUR_CMAKE_MODULES_DIR}/Boilerplate" "${OUR_CMAKE_MODULES_DIR}/DoxygenUtils" "${OUR_CMAKE_3PARTY_MODULES_DIR}")

include(Boilerplate)


set(Include_dir "${CMAKE_CURRENT_SOURCE_DIR}/include")
file(GLOB_RECURSE HDRFILES "${Include_dir}/*.h" "${Include_dir}/*.hpp")

set(LibSource_dir "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(PackagingTemplatesDir "${CMAKE_CURRENT_SOURCE_DIR}/packaging")
set(tests_dir "${CMAKE_CURRENT_SOURCE_DIR}/tests")

set(CPACK_DEBIAN_PACKAGE_NAME "${CPACK_PACKAGE_NAME}")
set(CPACK_RPM_PACKAGE_NAME "${CPACK_PACKAGE_NAME}")

option(WITH_TESTS ON "Enable testing")

if(WITH_TESTS)
	add_subdirectory("${tests_dir}")
	enable_testing()
endif()

add_subdirectory("${LibSource_dir}")

set(CPACK_PACKAGE_MAINTAINER "${CPACK_PACKAGE_VENDOR}")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "${CPACK_PACKAGE_MAINTAINER}")
set(CPACK_PACKAGE_MAINTAINER "${CPACK_PACKAGE_VENDOR}")
set(CPACK_DEB_COMPONENT_INSTALL ON)
set(CPACK_RPM_COMPONENT_INSTALL ON)
set(CPACK_NSIS_COMPONENT_INSTALL ON)
#set(CPACK_DEBIAN_ENABLE_COMPONENT_DEPENDS ON)
set(CPACK_RPM_ENABLE_COMPONENT_DEPENDS ON)
set(CPACK_DEBIAN_COMPRESSION_TYPE "xz")

include(CPack)
