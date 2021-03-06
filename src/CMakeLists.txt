cmake_minimum_required (VERSION 2.6)

# json-c pkgconfig is installed under /lib64 instead of /usr/lib64
# https://bugzilla.redhat.com/show_bug.cgi?id=1158842
set (ENV{PKG_CONFIG_PATH} "$ENV{PKG_CONFIG_PATH}:/lib64/pkgconfig:/lib/pkgconfig")

pkg_check_modules (GFAL2_PKG REQUIRED gfal2>=2.7.0)
pkg_check_modules (GLIB2_PKG REQUIRED glib-2.0)
pkg_check_modules (CURL_PKG REQUIRED libcurl)
pkg_check_modules (JSONC_PKG REQUIRED json)
pkg_check_modules (OPENSSL_PKG REQUIRED openssl)

include_directories (${GFAL2_PKG_INCLUDE_DIRS})
include_directories (${GLIB2_PKG_INCLUDE_DIRS})
include_directories (${CURL_PKG_INCLUDE_DIRS})
include_directories (${JSONC_PKG_INCLUDE_DIRS})
include_directories (${OPENSSL_PKG_INCLUDE_DIRS})

add_definitions (${GFAL2_PKG_CFLAGS})
add_definitions (${GLIB2_PKG_CFLAGS})
add_definitions (${CURL_PKG_CFLAGS})
add_definitions (${JSONC_PKG_CFLAGS})
add_definitions (${OPENSSL_PKG_CFLAGS})

file (GLOB src_zenodo "*.c")

add_library (gfal_plugin_zenodo SHARED ${src_zenodo})

target_link_libraries (gfal_plugin_zenodo ${GFAL2_PKG_LIBRARIES})
target_link_libraries (gfal_plugin_zenodo ${GLIB2_PKG_LIBRARIES})
target_link_libraries (gfal_plugin_zenodo ${CURL_PKG_LIBRARIES})
target_link_libraries (gfal_plugin_zenodo ${JSONC_PKG_LIBRARIES})
target_link_libraries (gfal_plugin_zenodo ${OPENSSL_PKG_LIBRARIES})

install (
    TARGETS gfal_plugin_zenodo
    LIBRARY DESTINATION ${PLUGIN_INSTALL_DIR}
)

#add_subdirectory (tests)
