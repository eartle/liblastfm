
find_package(Qt4 COMPONENTS QtCore QtSql REQUIRED)
#FIXME: find modules are horrible, port them to find_package_handle_standard_args
find_package(LibSamplerate REQUIRED)
find_package(LibFFTW3 REQUIRED)

include_directories(${LIBFFTW3_INCLUDE_DIR})
include_directories(${LIBSAMPLERATE_INCLUDE_DIR})
include_directories(${QT_INCLUDES})
include_directories(${CMAKE_CURRENT_LIST_DIR}/..)

set(lastfm_fingerprint_SOURCES
  Collection.cpp
  Fingerprint.cpp
  Sha256.cpp
  fplib/Filter.cpp
  fplib/FingerprintExtractor.cpp
  fplib/OptFFT.cpp
)

set(lastfm_fingerprint_HEADERS
    Fingerprint.h
    FingerprintableSource.h
)

add_library(lastfm_fingerprint SHARED ${lastfm_fingerprint_SOURCES})

target_link_libraries(lastfm_fingerprint
    lastfm
    ${QT_QTSQL_LIBRARY}
    ${QT_QTCORE_LIBRARY}
    ${LIBSAMPLERATE_LIBRARY}
    ${LIBFFTW3_LIBRARY}
)

set_target_properties(lastfm_fingerprint PROPERTIES
    COMPILE_DEFINITIONS "LASTFM_FINGERPRINT_LIB"
    VERSION ${LASTFM_VERSION}
    SOVERSION ${LASTFM_SOVERSION}
)

install(TARGETS lastfm_fingerprint
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(FILES ${lastfm_fingerprint_HEADERS} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/lastfm/)
