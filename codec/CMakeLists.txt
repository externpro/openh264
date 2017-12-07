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
if(CMAKE_BUILD_TYPE STREQUAL Debug)
  set(postfix ${CMAKE_DEBUG_POSTFIX})
elseif(CMAKE_BUILD_TYPE STREQUAL Release)
  set(postfix ${CMAKE_RELEASE_POSTFIX})
endif()
set(bin_name ${CMAKE_STATIC_LIBRARY_PREFIX}${lib_name}${ver}${postfix}${CMAKE_STATIC_LIBRARY_SUFFIX})
if(WIN32)
  # NOTE: combining libraries never tested on Windows...
  set(cmd lib.exe /OUT:${bin_name})
else()
  set(cmd ${CMAKE_AR} cqT ${bin_name})
  set(mri ${CMAKE_CURRENT_BINARY_DIR}/${lib_name}.mri)
  file(WRITE ${mri} "create ${bin_name}\naddlib ${bin_name}\nsave\nend")
endif()
foreach(lib ${lib_deps})
  list(APPEND cmd $<TARGET_FILE:${lib}>)
endforeach()
if(NOT WIN32)
  list(APPEND cmd COMMAND ${CMAKE_AR} -M <${mri})
endif()
set(lib_srcs
  api/svc/codec_api.h
  api/svc/codec_app_def.h
  api/svc/codec_def.h
  api/svc/codec_ver.h
  )
source_group(api\\svc FILES ${lib_srcs})
# https://stackoverflow.com/questions/14199708/cmake-include-library-dependencies-in-static-lib
# https://stackoverflow.com/questions/3821916/how-to-merge-two-ar-static-libraries-into-one
add_custom_command(OUTPUT ${bin_name} COMMAND ${cmd} DEPENDS ${lib_deps} COMMENT "Combining library ${bin_name}")
add_custom_target(${lib_name} ALL DEPENDS ${bin_name} SOURCES ${lib_srcs})
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${bin_name} DESTINATION lib)
install(FILES ${lib_srcs} DESTINATION include${verDir}/wels)
