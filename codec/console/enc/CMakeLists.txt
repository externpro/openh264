set(exe_name h264enc)
set(exe_deps encoder console_common)
####################
set(exe_srcs
  targets.mk
  src/welsenc.cpp
  )
source_group("" FILES ${exe_srcs})
####################
add_executable(${exe_name} ${exe_srcs})
target_link_libraries(${exe_name} ${exe_deps})
