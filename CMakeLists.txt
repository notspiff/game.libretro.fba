project(game.libretro.fba)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(Kodi REQUIRED)

if(CMAKE_BUILD_TYPE MATCHES Debug)
  set(LIBRETRO_DEBUG DEBUG=1)
endif()

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make -C svn-current/trunk -f makefile.libretro platform=win ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make -C svn-current/trunk -f makefile.libretro ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  set(BUILD_COMMAND make -C svn-current/trunk -f makefile.libretro platform=osx ${LIBRETRO_DEBUG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

include(ExternalProject)
externalproject_add(fba
                    GIT_REPOSITORY https://github.com/libretro/fba-libretro
                    GIT_TAG 7e70ddf8
                    PREFIX fba
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(FBA_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/fba/src/fba/svn-current/trunk/fb_alpha_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                      game.libretro.fba${CMAKE_SHARED_LIBRARY_SUFFIX} fba)

build_addon(game.libretro.fba FBA DEPLIBS)
add_dependencies(game.libretro.fba fba)

include(CPack)
