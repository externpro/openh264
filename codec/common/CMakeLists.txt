set(lib_name common)
set(obj_name ${lib_name}_obj)
####################
set(lib_srcs
  generate_version.sh
  targets.mk
  )
source_group("" FILES ${lib_srcs})
####################
set(arm_srcs
  arm/arm_arch_common_macro.S
  arm/copy_mb_neon.S
  arm/deblocking_neon.S
  arm/expand_picture_neon.S
  arm/intra_pred_common_neon.S
  arm/mc_neon.S
  )
source_group(arm FILES ${arm_srcs})
set(obj_srcs ${arm_srcs})
####################
set(arm64_srcs
  arm64/arm_arch64_common_macro.S
  arm64/copy_mb_aarch64_neon.S
  arm64/deblocking_aarch64_neon.S
  arm64/expand_picture_aarch64_neon.S
  arm64/intra_pred_common_aarch64_neon.S
  arm64/mc_aarch64_neon.S
  )
source_group(arm64 FILES ${arm64_srcs})
list(APPEND obj_srcs ${arm64_srcs})
####################
set(inc_srcs
  inc/copy_mb.h
  inc/cpu_core.h
  inc/cpu.h
  inc/crt_util_safe_x.h
  inc/deblocking_common.h
  inc/expand_pic.h
  inc/intra_pred_common.h
  inc/ls_defines.h
  inc/macros.h
  inc/mc.h
  inc/measure_time.h
  inc/memory_align.h
  inc/sad_common.h
  inc/typedefs.h
  inc/utils.h
  inc/version_gen.h.template
  inc/version.h
  inc/welsCodecTrace.h
  inc/wels_common_defs.h
  inc/wels_const_common.h
  inc/WelsThreadLib.h
  )
source_group(inc FILES ${inc_srcs})
list(APPEND obj_srcs ${inc_srcs})
####################
set(src_srcs
  src/common_tables.cpp
  src/copy_mb.cpp
  src/cpu.cpp
  src/crt_util_safe_x.cpp
  src/deblocking_common.cpp
  src/expand_pic.cpp
  src/intra_pred_common.cpp
  src/mc.cpp
  src/memory_align.cpp
  src/sad_common.cpp
  src/utils.cpp
  src/welsCodecTrace.cpp
  src/WelsThreadLib.cpp
  )
source_group(src FILES ${src_srcs})
list(APPEND obj_srcs ${src_srcs})
####################
set(x86_srcs
  x86/asm_inc.asm
  x86/cpuid.asm
  x86/deblock.asm
  x86/expand_picture.asm
  x86/intra_pred_com.asm
  x86/mb_copy.asm
  x86/mc_chroma.asm
  x86/mc_luma.asm
  x86/satd_sad.asm
  x86/vaa.asm
  )
source_group(x86 FILES ${x86_srcs})
list(APPEND obj_srcs ${x86_srcs})
####################
set(incDirs $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/inc> ${codec_SOURCE_DIR}/api/svc)
if(CMAKE_ASM_NASM_COMPILER_LOADED)
  list(APPEND incDirs $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/x86>)
endif()
##########
add_library(${obj_name} OBJECT ${obj_srcs})
target_include_directories(${obj_name} PRIVATE ${incDirs})
##########
add_library(${lib_name} STATIC ${lib_srcs} $<TARGET_OBJECTS:${obj_name}>)
target_include_directories(${lib_name} PUBLIC ${incDirs})
set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)
target_link_libraries(${lib_name} Threads::Threads)
