set(lib_name encoder)
set(lib_deps processing)
set(obj_name ${lib_name}_obj)
####################
set(lib_srcs
  targets.mk
  )
source_group("" FILES ${lib_srcs})
####################
set(core_arm_srcs
  core/arm/intra_pred_neon.S
  core/arm/intra_pred_sad_3_opt_neon.S
  core/arm/memory_neon.S
  core/arm/pixel_neon.S
  core/arm/reconstruct_neon.S
  core/arm/svc_motion_estimation.S
  )
source_group(core\\arm FILES ${core_arm_srcs})
set(obj_srcs ${core_arm_srcs})
####################
set(core_arm64_srcs
  core/arm64/intra_pred_aarch64_neon.S
  core/arm64/intra_pred_sad_3_opt_aarch64_neon.S
  core/arm64/memory_aarch64_neon.S
  core/arm64/pixel_aarch64_neon.S
  core/arm64/reconstruct_aarch64_neon.S
  core/arm64/svc_motion_estimation_aarch64_neon.S
  )
source_group(core\\arm64 FILES ${core_arm64_srcs})
list(APPEND obj_srcs ${core_arm64_srcs})
####################
set(core_inc_srcs
  core/inc/as264_common.h
  core/inc/au_set.h
  core/inc/bit_stream.h
  core/inc/deblocking.h
  core/inc/decode_mb_aux.h
  core/inc/dq_map.h
  core/inc/encode_mb_aux.h
  core/inc/encoder_context.h
  core/inc/encoder.h
  core/inc/extern.h
  core/inc/get_intra_predictor.h
  core/inc/mb_cache.h
  core/inc/md.h
  core/inc/mt_defs.h
  core/inc/mv_pred.h
  core/inc/nal_encap.h
  core/inc/nal_prefix.h
  core/inc/parameter_sets.h
  core/inc/param_svc.h
  core/inc/picture.h
  core/inc/picture_handle.h
  core/inc/property.h
  core/inc/rc.h
  core/inc/ref_list_mgr_svc.h
  core/inc/sample.h
  core/inc/set_mb_syn_cabac.h
  core/inc/set_mb_syn_cavlc.h
  core/inc/slice.h
  core/inc/slice_multi_threading.h
  core/inc/stat.h
  core/inc/svc_base_layer_md.h
  core/inc/svc_enc_frame.h
  core/inc/svc_enc_golomb.h
  core/inc/svc_enc_macroblock.h
  core/inc/svc_encode_mb.h
  core/inc/svc_encode_slice.h
  core/inc/svc_enc_slice_segment.h
  core/inc/svc_mode_decision.h
  core/inc/svc_motion_estimate.h
  core/inc/svc_set_mb_syn_cavlc.h
  core/inc/svc_set_mb_syn.h
  core/inc/vlc_encoder.h
  core/inc/wels_common_basis.h
  core/inc/wels_const.h
  core/inc/wels_func_ptr_def.h
  core/inc/wels_preprocess.h
  core/inc/wels_transpose_matrix.h
  )
source_group(core\\inc FILES ${core_inc_srcs})
list(APPEND obj_srcs ${core_inc_srcs})
####################
set(core_src_srcs
  core/src/au_set.cpp
  core/src/deblocking.cpp
  core/src/decode_mb_aux.cpp
  core/src/encode_mb_aux.cpp
  core/src/encoder.cpp
  core/src/encoder_data_tables.cpp
  core/src/encoder_ext.cpp
  core/src/get_intra_predictor.cpp
  core/src/md.cpp
  core/src/mv_pred.cpp
  core/src/nal_encap.cpp
  core/src/picture_handle.cpp
  core/src/property.cpp
  core/src/ratectl.cpp
  core/src/ref_list_mgr_svc.cpp
  core/src/sample.cpp
  core/src/set_mb_syn_cabac.cpp
  core/src/set_mb_syn_cavlc.cpp
  core/src/slice_multi_threading.cpp
  core/src/svc_base_layer_md.cpp
  core/src/svc_encode_mb.cpp
  core/src/svc_encode_slice.cpp
  core/src/svc_enc_slice_segment.cpp
  core/src/svc_mode_decision.cpp
  core/src/svc_motion_estimate.cpp
  core/src/svc_set_mb_syn_cabac.cpp
  core/src/svc_set_mb_syn_cavlc.cpp
  core/src/wels_preprocess.cpp
  )
source_group(core\\src FILES ${core_src_srcs})
list(APPEND obj_srcs ${core_src_srcs})
####################
set(core_x86_srcs
  core/x86/coeff.asm
  core/x86/dct.asm
  core/x86/intra_pred.asm
  core/x86/matrix_transpose.asm
  core/x86/memzero.asm
  core/x86/quant.asm
  core/x86/sample_sc.asm
  core/x86/score.asm
  )
source_group(core\\x86 FILES ${core_x86_srcs})
list(APPEND obj_srcs ${core_x86_srcs})
####################
set(plus_srcs
  plus/inc/welsEncoderExt.h
  plus/src/welsEncoderExt.cpp
  )
source_group(plus FILES ${plus_srcs})
list(APPEND obj_srcs ${plus_srcs})
####################
set(plus_srcs
  plus/res/resource.h
  plus/res/welsenc.rc
  #plus/src/DllEntry.cpp
  plus/src/wels_enc_export.def
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
