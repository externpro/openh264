set(lib_name processing)
set(lib_deps common)
####################
set(lib_srcs
  targets.mk
  )
source_group("" FILES ${lib_srcs})
####################
set(interface_srcs
  interface/IWelsVP.h
  )
source_group(interface FILES ${interface_srcs})
list(APPEND lib_srcs ${interface_srcs})
####################
set(src_adaptivequantization_srcs
  src/adaptivequantization/AdaptiveQuantization.cpp
  src/adaptivequantization/AdaptiveQuantization.h
  )
source_group(src\\adaptivequantization FILES ${src_adaptivequantization_srcs})
list(APPEND lib_srcs ${src_adaptivequantization_srcs})
####################
set(src_arm_srcs
  src/arm/adaptive_quantization.S
  src/arm/down_sample_neon.S
  src/arm/pixel_sad_neon.S
  src/arm/vaa_calc_neon.S
  )
source_group(src\\arm FILES ${src_arm_srcs})
list(APPEND lib_srcs ${src_arm_srcs})
####################
set(src_arm64_srcs
  src/arm64/adaptive_quantization_aarch64_neon.S
  src/arm64/down_sample_aarch64_neon.S
  src/arm64/pixel_sad_aarch64_neon.S
  src/arm64/vaa_calc_aarch64_neon.S
  )
source_group(src\\arm64 FILES ${src_arm64_srcs})
list(APPEND lib_srcs ${src_arm64_srcs})
####################
set(src_backgrounddetection_srcs
  src/backgrounddetection/BackgroundDetection.cpp
  src/backgrounddetection/BackgroundDetection.h
  )
source_group(src\\backgrounddetection FILES ${src_backgrounddetection_srcs})
list(APPEND lib_srcs ${src_backgrounddetection_srcs})
####################
set(src_common_srcs
  src/common/common.h
  src/common/memory.cpp
  src/common/memory.h
  src/common/resource.h
  src/common/typedef.h
  src/common/util.h
  src/common/WelsFrameWork.cpp
  src/common/WelsFrameWorkEx.cpp
  src/common/WelsFrameWork.h
  src/common/WelsVP.def
  src/common/WelsVP.rc
  )
source_group(src\\common FILES ${src_common_srcs})
list(APPEND lib_srcs ${src_common_srcs})
####################
set(src_complexityanalysis_srcs
  src/complexityanalysis/ComplexityAnalysis.cpp
  src/complexityanalysis/ComplexityAnalysis.h
  )
source_group(src\\complexityanalysis FILES ${src_complexityanalysis_srcs})
list(APPEND lib_srcs ${src_complexityanalysis_srcs})
####################
set(src_denoise_srcs
  src/denoise/denoise.cpp
  src/denoise/denoise_filter.cpp
  src/denoise/denoise.h
  )
source_group(src\\denoise FILES ${src_denoise_srcs})
list(APPEND lib_srcs ${src_denoise_srcs})
####################
set(src_downsample_srcs
  src/downsample/downsample.cpp
  src/downsample/downsamplefuncs.cpp
  src/downsample/downsample.h
  )
source_group(src\\downsample FILES ${src_downsample_srcs})
list(APPEND lib_srcs ${src_downsample_srcs})
####################
set(src_imagerotate_srcs
  src/imagerotate/imagerotate.cpp
  src/imagerotate/imagerotatefuncs.cpp
  src/imagerotate/imagerotate.h
  )
source_group(src\\imagerotate FILES ${src_imagerotate_srcs})
list(APPEND lib_srcs ${src_imagerotate_srcs})
####################
set(src_scenechangedetection_srcs
  src/scenechangedetection/SceneChangeDetection.cpp
  src/scenechangedetection/SceneChangeDetection.h
  )
source_group(src\\scenechangedetection FILES ${src_scenechangedetection_srcs})
list(APPEND lib_srcs ${src_scenechangedetection_srcs})
####################
set(src_scrolldetection_srcs
  src/scrolldetection/ScrollDetection.cpp
  src/scrolldetection/ScrollDetectionFuncs.cpp
  src/scrolldetection/ScrollDetectionFuncs.h
  src/scrolldetection/ScrollDetection.h
  )
source_group(src\\scrolldetection FILES ${src_scrolldetection_srcs})
list(APPEND lib_srcs ${src_scrolldetection_srcs})
####################
set(src_vaacalc_srcs
  src/vaacalc/vaacalcfuncs.cpp
  src/vaacalc/vaacalculation.cpp
  src/vaacalc/vaacalculation.h
  )
source_group(src\\vaacalc FILES ${src_vaacalc_srcs})
list(APPEND lib_srcs ${src_vaacalc_srcs})
####################
set(src_x86_srcs
  src/x86/denoisefilter.asm
  src/x86/downsample_bilinear.asm
  src/x86/vaa.asm
  )
source_group(src\\x86 FILES ${src_x86_srcs})
list(APPEND lib_srcs ${src_x86_srcs})
####################
add_library(${lib_name} STATIC ${lib_srcs})
target_include_directories(${lib_name} PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/interface>
  PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src/common
  )
target_link_libraries(${lib_name} ${lib_deps})
