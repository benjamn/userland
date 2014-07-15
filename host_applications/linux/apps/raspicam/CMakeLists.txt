
# raspistill/raspivid/raspiyuv

SET(COMPILE_DEFINITIONS -Werror)

include_directories(${CMAKE_SOURCE_DIR}/host_applications/linux/libs/bcm_host/include)
include_directories(${CMAKE_SOURCE_DIR}/host_applications/linux/apps/raspicam/)

set (GL_SCENE_SOURCES
   gl_scenes/models.c
   gl_scenes/mirror.c
   gl_scenes/yuv.c
   gl_scenes/sobel.c
   gl_scenes/offgrid.c
   gl_scenes/square.c
   gl_scenes/teapot.c)

set (COMMON_SOURCES
   RaspiCamControl.c
   RaspiCLI.c
   RaspiPreview.c)

add_executable(raspistill ${COMMON_SOURCES} RaspiStill.c  RaspiTex.c RaspiTexUtil.c tga.c ${GL_SCENE_SOURCES})
add_executable(raspiyuv   ${COMMON_SOURCES} RaspiStillYUV.c)
add_executable(raspivid   ${COMMON_SOURCES} RaspiVid.c)
add_executable(offgrid
  ${COMMON_SOURCES}
  OffGrid.c
  RaspiTex.c
  RaspiTexUtil.c
  tga.c
  tclled.c
  ${GL_SCENE_SOURCES})

set (MMAL_LIBS mmal_core mmal_util mmal_vc_client)

target_link_libraries(raspistill ${MMAL_LIBS} vcos bcm_host GLESv2 EGL m)
target_link_libraries(raspiyuv   ${MMAL_LIBS} vcos bcm_host)
target_link_libraries(raspivid   ${MMAL_LIBS} vcos bcm_host)
target_link_libraries(offgrid    ${MMAL_LIBS} vcos bcm_host GLESv2 EGL m)

install(TARGETS raspistill raspiyuv raspivid offgrid RUNTIME DESTINATION bin)
