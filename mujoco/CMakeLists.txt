cmake_minimum_required(VERSION 3.5)
set(MUJOCO_VERSION 3.0.0)
project(mujoco VERSION ${MUJOCO_VERSION})

find_package(catkin REQUIRED)

include(ExternalProject)

ExternalProject_Add(
  mujoco
  GIT_REPOSITORY https://github.com/google-deepmind/mujoco.git
  GIT_TAG ${MUJOCO_VERSION}
  UPDATE_COMMAND ""
  SOURCE_DIR "mujoco-src"
  BINARY_DIR "mujoco-build"
  INSTALL_COMMAND ""
  CMAKE_ARGS -DMUJOCO_BUILD_TESTS=OFF)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/${PROJECT_NAME})
catkin_package(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include/${PROJECT_NAME}
               LIBRARIES mujoco)
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory mujoco-build/bin
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION})
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy mujoco-build/lib/*mujoco.so
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_LIB_DESTINATION})
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND
    ${CMAKE_COMMAND} -E copy mujoco-build/lib/*mujoco.so.${MUJOCO_VERSION}
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_LIB_DESTINATION})
file(MAKE_DIRECTORY
     ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/mujoco_plugin)
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND
    ${CMAKE_COMMAND} -E copy mujoco-build/lib/*.so
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/mujoco_plugin &&
    ${CMAKE_COMMAND} -E remove
    ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_BIN_DESTINATION}/mujoco_plugin/*mujoco.so
)
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory mujoco-src/include
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION})
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory mujoco-src/model
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/model)
