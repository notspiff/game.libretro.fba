project(game.libretro.fba)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

include(ExternalProject)
externalproject_add(fba
                    GIT_REPOSITORY https://github.com/libretro/fba-libretro
                    GIT_TAG f16b316
                    PREFIX fba
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND <SOURCE_DIR>/compile_libretro.sh make
                    BUILD_IN_SOURCE 1)

set(FBA_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/fba/src/fba/svn-current/trunk/fb_alpha_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                      game.libretro.fba${CMAKE_SHARED_LIBRARY_SUFFIX} fba)

build_addon(game.libretro.fba FBA DEPLIBS)
add_dependencies(game.libretro.fba fba)

include(CPack)
