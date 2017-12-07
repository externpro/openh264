set(lib_name console_common)
####################
set(lib_srcs
  targets.mk
  inc/read_config.h
  src/read_config.cpp
  )
source_group("" FILES ${lib_srcs})
####################
add_library(${lib_name} STATIC ${lib_srcs})
target_include_directories(${lib_name} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/inc>)
