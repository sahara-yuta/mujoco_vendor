cmake_minimum_required(VERSION 3.5)
project(mujoco_vendor VERSION 3.0.0)

find_package(catkin REQUIRED)

include(ExternalProject)
ExternalProject_Add(
  mujoco
  GIT_REPOSITORY git@github.com:google-deepmind/mujoco.git
  GIT_TAG 3.0.0
  UPDATE_COMMAND ""
  SOURCE_DIR "mujoco-src"
  BINARY_DIR "mujoco-build"
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX})

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)
catkin_package(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include LIBRARIES mujoco)

add_custom_command(
  TARGET mujoco
  POST_BUILD
  COMMAND
    ${CMAKE_COMMAND} -E echo "move bin" && ${CMAKE_COMMAND} -E copy_directory
    ${CATKIN_DEVEL_PREFIX}/bin ${CATKIN_DEVEL_PREFIX}/lib/${PROJECT_NAME})
