cmake_minimum_required(VERSION 4.3)
project(openh264)
set(targetsFile ${PROJECT_NAME}-targets)
if(NOT DEFINED CMAKE_INSTALL_CMAKEDIR)
  set(CMAKE_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
endif()
if(COMMAND xpGetVersionString)
  set(prefix @openh264_DIR@/../..)
  set(lib_dir ${CMAKE_INSTALL_LIBDIR})
  set(inc_dir ${CMAKE_INSTALL_INCLUDEDIR})
  xpGetVersionString(verString)
  set(VERSION ${verString})
  set(LIBS -lstdc++)
  set(pcFile ${CMAKE_PROJECT_NAME}.pc.in)
  configure_file(${pcFile} ${CMAKE_CURRENT_BINARY_DIR} @ONLY NEWLINE_STYLE LF)
  install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${pcFile} DESTINATION ${CMAKE_INSTALL_CMAKEDIR})
endif()
add_subdirectory(codec)
if(COMMAND xpExternPackage)
  xpExternPackage(TARGETS_FILE ${targetsFile}
    LIBRARIES ${PROJECT_NAME} DEFAULT_TARGETS ${PROJECT_NAME}
    BASE v1.4.0 XPDIFF "intro" DEPS Threads PVT_DEPS yasm
    WEB "http://www.openh264.org/" UPSTREAM "github.com/cisco/openh264"
    DESC "a codec library which supports H.264 encoding and decoding"
    LICENSE "[BSD-2-Clause](http://www.openh264.org/faq.html 'BSD 2-Clause Simplified License')"
    )
endif()
