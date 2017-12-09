set(lib_name decoder)
set(lib_deps common)
set(obj_name ${lib_name}_obj)
####################
set(lib_srcs
  targets.mk
  )
source_group("" FILES ${lib_srcs})
####################
set(core_arm_srcs
  core/arm/block_add_neon.S
  core/arm/intra_pred_neon.S
  )
source_group(core\\arm FILES ${core_arm_srcs})
set(obj_srcs ${core_arm_srcs})
####################
set(core_arm64_srcs
  core/arm64/block_add_aarch64_neon.S
  core/arm64/intra_pred_aarch64_neon.S
  )
source_group(core\\arm64 FILES ${core_arm64_srcs})
list(APPEND obj_srcs ${core_arm64_srcs})
####################
set(core_inc_srcs
  core/inc/as264_common.h
  core/inc/au_parser.h
  core/inc/bit_stream.h
  core/inc/cabac_decoder.h
  core/inc/deblocking.h
  core/inc/dec_frame.h
  core/inc/dec_golomb.h
  core/inc/decode_mb_aux.h
  core/inc/decoder_context.h
  core/inc/decoder_core.h
  core/inc/decoder.h
  core/inc/decode_slice.h
  core/inc/error_code.h
  core/inc/error_concealment.h
  core/inc/fmo.h
  core/inc/get_intra_predictor.h
  core/inc/manage_dec_ref.h
  core/inc/mb_cache.h
  core/inc/memmgr_nal_unit.h
  core/inc/mv_pred.h
  core/inc/nal_prefix.h
  core/inc/nalu.h
  core/inc/parameter_sets.h
  core/inc/parse_mb_syn_cabac.h
  core/inc/parse_mb_syn_cavlc.h
  core/inc/pic_queue.h
  core/inc/picture.h
  core/inc/rec_mb.h
  core/inc/slice.h
  core/inc/vlc_decoder.h
  core/inc/wels_common_basis.h
  core/inc/wels_const.h
  )
source_group(core\\inc FILES ${core_inc_srcs})
list(APPEND obj_srcs ${core_inc_srcs})
####################
set(core_src_srcs
  core/src/au_parser.cpp
  core/src/bit_stream.cpp
  core/src/cabac_decoder.cpp
  core/src/deblocking.cpp
  core/src/decode_mb_aux.cpp
  core/src/decoder_core.cpp
  core/src/decoder.cpp
  core/src/decoder_data_tables.cpp
  core/src/decode_slice.cpp
  core/src/error_concealment.cpp
  core/src/fmo.cpp
  core/src/get_intra_predictor.cpp
  core/src/manage_dec_ref.cpp
  core/src/memmgr_nal_unit.cpp
  core/src/mv_pred.cpp
  core/src/parse_mb_syn_cabac.cpp
  core/src/parse_mb_syn_cavlc.cpp
  core/src/pic_queue.cpp
  core/src/rec_mb.cpp
  )
source_group(core\\src FILES ${core_src_srcs})
list(APPEND obj_srcs ${core_src_srcs})
####################
set(core_x86_srcs
  core/x86/dct.asm
  core/x86/intra_pred.asm
  )
source_group(core\\x86 FILES ${core_x86_srcs})
list(APPEND obj_srcs ${core_x86_srcs})
####################
set(plus_srcs
  plus/inc/welsDecoderExt.h
  plus/src/welsDecoderExt.cpp
  )
source_group(plus FILES ${plus_srcs})
list(APPEND obj_srcs ${plus_srcs})
####################
set(plus_srcs
  plus/res/resource.h
  plus/res/welsdec.rc
  )
source_group(plus FILES ${plus_srcs})
list(APPEND lib_srcs ${plus_srcs})
####################
set(incDirs
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/core/inc>
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/plus/inc>
  )
##########
foreach(lib ${lib_deps})
  list(APPEND depInc $<TARGET_PROPERTY:${lib},INTERFACE_INCLUDE_DIRECTORIES>)
endforeach()
add_library(${obj_name} OBJECT ${obj_srcs})
target_include_directories(${obj_name} PRIVATE ${incDirs} ${depInc})
##########
add_library(${lib_name} STATIC ${lib_srcs} $<TARGET_OBJECTS:${obj_name}>)
target_include_directories(${lib_name} PUBLIC ${incDirs})
target_link_libraries(${lib_name} ${lib_deps})
