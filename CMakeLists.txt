cmake_minimum_required(VERSION 3.28)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/.devcontainer/cmake)
include(preproject)
project(nasm VERSION 2.14.02)
set(buildpro_REV 24.07)
include(flags)
include(GNUInstallDirs)
set(XP_INSTALL_CMAKEDIR ${CMAKE_INSTALL_DATADIR}/cmake)
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
  xpPackageDevel(EXE_PATH bin/nasm)
  ExternalProject_Get_Property(${PROJECT_NAME} INSTALL_DIR)
  install(DIRECTORY ${INSTALL_DIR}/ DESTINATION . USE_SOURCE_PERMISSIONS)
else()
  xpPackageDevel()
endif()
