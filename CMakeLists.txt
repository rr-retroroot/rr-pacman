cmake_minimum_required(VERSION 3.0)

########################
# PROJECT SETUP
########################
project(rr-pacman)
set(VERSION_MAJOR "1")
set(VERSION_MINOR "0")
set(PROJECT_AUTHOR "cpasjuste")

########################
# SOURCES SETUP
########################
file(GLOB SOURCES
        src/common/*.c
        src/pacman/*.c
        lib/libalpm/*.c
        )

set(INCLUDES
        src/common
        src/pacman
        lib/libalpm
        )

set(CFLAGS
        -D__RETROROOT__
        -DCACHEDIR="/opt/pacbrew/pacman/var/cache/pacman/pkg/"
        -DCONFFILE="/opt/pacbrew/pacman/etc/pacman.conf"
        -DDBPATH="/opt/pacbrew/pacman/var/lib/pacman/"
        -DENABLE_NLS=1
        -DGPGDIR="/opt/pacbrew/pacman/etc/pacman.d/gnupg/"
        -DHAVE_GETMNTENT=1
        -DHAVE_LIBCURL
        -DHAVE_LIBGPGME
        -DHAVE_LIBSSL=1
        -DHAVE_MNTENT_H
        -DHAVE_STRNDUP=1
        -DHAVE_STRNLEN=1
        -DHAVE_STRSEP=1
        -DHAVE_STRUCT_STATVFS_F_FLAG
        -DHAVE_STRUCT_STAT_ST_BLKSIZE
        -DHAVE_SWPRINTF=1
        -DHAVE_SYS_MOUNT_H
        -DHAVE_SYS_PARAM_H
        -DHAVE_SYS_STATVFS_H
        -DHAVE_SYS_TYPES_H
        -DHAVE_TCFLUSH=1
        -DHAVE_TERMIOS_H
        -DHOOKDIR="/opt/pacbrew/pacman/etc/pacman.d/hooks/"
        -DLDCONFIG="/sbin/ldconfig"
        -DLIB_VERSION="13.0.1"
        -DLOCALEDIR="/opt/pacbrew/pacman/share/locale"
        -DLOGFILE="/opt/pacbrew/pacman/var/log/pacman.log"
        -DPACKAGE="pacman"
        -DPACKAGE_VERSION="6.0.1"
        -DPACMAN_DEBUG=1
        -DROOTDIR="/"
        -DSCRIPTLET_SHELL="/bin/sh"
        -DSYSHOOKDIR="/opt/pacbrew/pacman/share/libalpm/hooks/"
        -D_GNU_SOURCE
        )

set(LDFLAGS)

##############
# deps
##############
include(FindPkgConfig)
pkg_search_module(LIBARCHIVE REQUIRED libarchive)
pkg_search_module(GPGME REQUIRED gpgme)
list(APPEND LDFLAGS ${LIBARCHIVE_LIBRARIES} ${GPGME_LIBRARIES})

find_package(CURL REQUIRED)
list(APPEND LDFLAGS ${CURL_LIBRARIES})

#####################
# LIBRARY
#####################
add_library(rr-pacman ${SOURCES})
target_include_directories(rr-pacman PUBLIC ${INCLUDES})
target_compile_options(rr-pacman PUBLIC ${CFLAGS})
target_link_libraries(rr-pacman PUBLIC ${LDFLAGS})
