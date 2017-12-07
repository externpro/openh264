set(exe_name h264dec)
set(exe_deps decoder console_common)
####################
set(exe_srcs
  targets.mk
  inc/d3d9_utils.h
  src/d3d9_utils.cpp
  src/h264dec.cpp
  )
source_group("" FILES ${exe_srcs})
####################
add_executable(${exe_name} ${exe_srcs})
target_include_directories(${exe_name} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/inc)
target_link_libraries(${exe_name} ${exe_deps})
