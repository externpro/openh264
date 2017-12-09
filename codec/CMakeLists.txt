project(codec)
if(${CMAKE_SYSTEM_PROCESSOR} MATCHES "x86")
  enable_language(ASM_NASM)
  if(CMAKE_ASM_NASM_COMPILER_LOADED)
    # TRICKY: NASM needs trailing slash https://gitlab.kitware.com/cmake/cmake/issues/12919
    set(asmInc -I${CMAKE_CURRENT_SOURCE_DIR}/common/x86/)
    # TRICKY: CMAKE_ASM_NASM_COMPILE_OBJECT is defined in CMakeASM_NASMInformation.cmake as
    # <CMAKE_ASM_NASM_COMPILER> <INCLUDES> <FLAGS> -f ${CMAKE_ASM_NASM_OBJECT_FORMAT} -o <OBJECT> <SOURCE>
    string(REPLACE "<INCLUDES> <FLAGS>" "${asmInc} -DUNIX64"
      CMAKE_ASM_NASM_COMPILE_OBJECT ${CMAKE_ASM_NASM_COMPILE_OBJECT}
      )
    add_definitions(-DX86_ASM)
  endif()
endif()
add_subdirectory(common)
add_subdirectory(decoder)
add_subdirectory(processing)
add_subdirectory(encoder)
add_subdirectory(console)
####################
set(lib_name openh264)
set(lib_deps common decoder processing encoder)
set(lib_srcs
  api/svc/codec_api.h
  api/svc/codec_app_def.h
  api/svc/codec_def.h
  api/svc/codec_ver.h
  )
source_group(api\\svc FILES ${lib_srcs})
##########
foreach(lib ${lib_deps})
  list(APPEND obj_libs $<TARGET_OBJECTS:${lib}_obj>)
endforeach()
add_library(${lib_name} STATIC ${lib_srcs} ${obj_libs})
set_target_properties(${lib_name} PROPERTIES OUTPUT_NAME ${lib_name}${ver})
##########
set(targetsFile ${lib_name}${ver}-targets)
install(TARGETS ${lib_name} EXPORT ${targetsFile}
  RUNTIME DESTINATION bin
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  )
install(FILES ${lib_srcs} DESTINATION include${verDir}/wels)
install(EXPORT ${targetsFile} DESTINATION lib/cmake)
