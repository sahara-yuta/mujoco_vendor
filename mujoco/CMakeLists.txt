cmake_minimum_required(VERSION 3.5)
project(mujoco VERSION 3.0.0)

find_package(catkin REQUIRED)

include(ExternalProject)

ExternalProject_Add(
  mujoco
  GIT_REPOSITORY git@github.com:google-deepmind/mujoco.git
  GIT_TAG 3.0.0
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
  COMMAND ${CMAKE_COMMAND} -E copy mujoco-build/lib/*.so*
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_LIB_DESTINATION})
add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy_directory mujoco-src/include
          ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION})
