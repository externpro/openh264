cmake_minimum_required(VERSION 3.31)
set(CMAKE_PROJECT_TOP_LEVEL_INCLUDES .devcontainer/cmake/xproinc.cmake)
project(openh264)
set(targetsFile ${PROJECT_NAME}-targets)
if(DEFINED XP_NAMESPACE)
  xpExternPackage(NAMESPACE ${XP_NAMESPACE}
    TARGETS_FILE ${targetsFile} LIBRARIES ${PROJECT_NAME}
    BASE v1.4.0 XPDIFF "intro" PVT_DEPS yasm
    WEB "http://www.openh264.org/" UPSTREAM "github.com/cisco/openh264"
    DESC "a codec library which supports H.264 encoding and decoding"
    LICENSE "[BSD-2-Clause](http://www.openh264.org/faq.html 'BSD 2-Clause Simplified License')"
    )
  set(prefix @xpuse-openh264_DIR@/../..)
  set(lib_dir ${CMAKE_INSTALL_LIBDIR})
  set(inc_dir ${CMAKE_INSTALL_INCLUDEDIR})
  xpGetVersionString(verString)
  set(VERSION ${verString})
  set(LIBS -lstdc++)
  set(pcFile ${CMAKE_PROJECT_NAME}.pc.in)
  configure_file(${pcFile} ${CMAKE_CURRENT_BINARY_DIR} @ONLY NEWLINE_STYLE LF)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${pcFile} DESTINATION ${CMAKE_INSTALL_CMAKEDIR})
  set(nameSpace NAMESPACE ${XP_NAMESPACE}::)
elseif(NOT DEFINED CMAKE_INSTALL_CMAKEDIR)
  set(CMAKE_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
add_subdirectory(codec)
