cmake_minimum_required(VERSION 4.3)
project(nasm VERSION 2.14.02)
include(ExternalProject)
set_property(DIRECTORY PROPERTY "EP_BASE" ${CMAKE_BINARY_DIR}/epbase)
if(WIN32 AND NOT UNIX)
  set(VER ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH})
  set(OS win64)
  ExternalProject_Add(${PROJECT_NAME}
    DOWNLOAD_EXTRACT_TIMESTAMP true
    URL https://www.nasm.us/pub/${PROJECT_NAME}/releasebuilds/${VER}/${OS}/${PROJECT_NAME}-${VER}-${OS}.zip
    URL_HASH SHA256=18918ac906e29417b936466e7a2517068206c8db8c04b9762a5befa18bfea5f0
    CONFIGURE_COMMAND ${CMAKE_COMMAND} -E copy_if_different
      <SOURCE_DIR>/nasm.exe <INSTALL_DIR>/bin/nasm.exe
    BUILD_COMMAND ${CMAKE_COMMAND} -E copy_if_different
      <SOURCE_DIR>/LICENSE <INSTALL_DIR>/share/LICENSE
    INSTALL_COMMAND ""
    )
  set(exePath EXE_PATH bin/nasm)
  ExternalProject_Get_Property(${PROJECT_NAME} INSTALL_DIR)
  install(DIRECTORY ${INSTALL_DIR}/ DESTINATION . USE_SOURCE_PERMISSIONS)
endif()
xpExternPackage(${exePath} BASE v0 XPDIFF "bin"
  WEB "https://www.nasm.us/"
  DESC "The Netwide Assembler - an 80x86 and x86-64 assembler (MSW-only)"
  LICENSE "[BSD-2-Clause](https://www.nasm.us/ 'BSD 2-Clause Simplified License')"
  )
