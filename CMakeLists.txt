cmake_minimum_required(VERSION 2.8)

# VitaSDK defines
if( NOT DEFINED CMAKE_TOOLCHAIN_FILE )
  if( DEFINED ENV{VITASDK} )
    set(CMAKE_TOOLCHAIN_FILE "$ENV{VITASDK}/share/vita.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define VITASDK to point to your SDK path!")
  endif()
endif()

# Project start
set(VITA_APPNAME FadeToBlack)
set(VITA_TITLEID "F2BGLVITA")

project(${VITA_APPNAME})
include("${VITASDK}/share/vita.cmake" REQUIRED)

# Flags and includes
set(VITA_MKSFOEX_FLAGS "-d ATTRIBUTE2=12")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -g -mtune=cortex-a9 -fno-exceptions")

include_directories(
  ${VITASDK}/arm-vita-eabi/include/SDL2
)

link_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
)

# Builds
add_executable(${VITA_APPNAME}
  cabinet.cpp
  camera.cpp
  collision.cpp
  cutscene.cpp
  decoder.cpp
  file.cpp
  font.cpp
  game.cpp
  icons.cpp
  input.cpp
  installer.cpp
  inventory.cpp
  main.cpp
  menu.cpp
  mixer.cpp
  opcodes.cpp
  raycast.cpp
  render.cpp
  resource.cpp
  saveload.cpp
  scaler.cpp
  screenshot.cpp
  sound.cpp
  spritecache.cpp
  stub.cpp
  texturecache.cpp
  trigo.cpp
  util.cpp
  xmiplayer.cpp
)

target_link_libraries(${VITA_APPNAME}
  SDL2
  vita2d
  vitaGL
  vitashark
  mathneon
  WildMidi
  z
  SceAppMgr_stub
  SceAppUtil_stub
  SceAudio_stub
  SceCommonDialog_stub
  SceCtrl_stub
  SceDisplay_stub
  SceGxm_stub
  SceHid_stub
  ScePower_stub
  SceShaccCg_stub
  SceSysmodule_stub
  SceTouch_stub
)

# Create Vita artifacts
vita_create_self(eboot.bin ${VITA_APPNAME} UNSAFE)
vita_create_vpk(${VITA_APPNAME}.vpk ${VITA_TITLEID} eboot.bin
  NAME ${VITA_APPNAME}
  FILE res/icon0.png sce_sys/icon0.png
  FILE res/bg.png sce_sys/livearea/contents/bg.png
  FILE res/startup.png sce_sys/livearea/contents/startup.png
  FILE res/template.xml sce_sys/livearea/contents/template.xml
  FILE res/shaders/replace_alpha_f.gxp shaders/replace_alpha_f.gxp
  FILE res/shaders/replace_alpha_fog_f.gxp shaders/replace_alpha_fog_f.gxp
  FILE res/shaders/texture2d_fog_v.gxp shaders/texture2d_fog_v.gxp
  FILE res/shaders/texture2d_v.gxp shaders/texture2d_v.gxp
  FILE res/shaders/vertex_f.gxp shaders/vertex_f.gxp
  FILE res/shaders/vertex_fog_f.gxp shaders/vertex_fog_f.gxp
  FILE res/shaders/vertex_fog_v.gxp shaders/vertex_fog_v.gxp
  FILE res/shaders/vertex_v.gxp shaders/vertex_v.gxp
)
