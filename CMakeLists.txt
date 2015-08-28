cmake_minimum_required(VERSION 2.8.3)
project(ORB_SLAM)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
    roscpp
    rospy
    sensor_msgs
    std_msgs
    tf
    image_transport
    cv_bridge
    cmake_modules
    )

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

find_package(OpenCV REQUIRED)
find_package(Eigen REQUIRED)
find_package(Boost REQUIRED)
find_package(Cholmod REQUIRED)

## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend and a run_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependencies might have been
##     pulled in transitively but can be declared for certainty nonetheless:
##     * add a build_depend tag for "message_generation"
##     * add a run_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#   FILES
#   Message1.msg
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
# generate_messages(
#   DEPENDENCIES
#   sensor_msgs#   std_msgs
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if you package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
    INCLUDE_DIRS include
    #  LIBRARIES ORB_SLAM
    CATKIN_DEPENDS roscpp rospy sensor_msgs std_msgs tf cmake_modules
    DEPENDS OpenCV cholmod
    )

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
    ${Eigen_INCLUDE_DIRS}
    ${Boost_INCLUDE_DIRS}
    ${CHOLMOD_INCLUDE_DIR}
    ${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/DBoW2
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/g20/core
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/g20/solvers
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/g20/stuff
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/g20/types
    )

## Declare a cpp library
# add_library(ORB_SLAM
#   src/${PROJECT_NAME}/ORB_SLAM.cpp
# )

## Declare a cpp executable
# add_executable(ORB_SLAM_node src/ORB_SLAM_node.cpp)
add_executable(ORB_SLAM 
    src/main.cc
    src/Tracking.cc
    src/LocalMapping.cc
    src/LoopClosing.cc
    src/ORBextractor.cc
    src/ORBmatcher.cc
    src/FramePublisher.cc
    src/Converter.cc
    src/MapPoint.cc
    src/KeyFrame.cc
    src/Map.cc
    src/MapPublisher.cc
    src/Optimizer.cc
    src/PnPsolver.cc
    src/Frame.cc
    src/KeyFrameDatabase.cc
    src/Sim3Solver.cc
    src/Initializer.cc)

## Add cmake target dependencies of the executable/library
## as an example, message headers may need to be generated before nodes
# add_dependencies(ORB_SLAM_node ORB_SLAM_generate_messages_cpp)

## Specify libraries to link a library or executable target against
# target_link_libraries(ORB_SLAM_node
#   ${catkin_LIBRARIES}
# )
target_link_libraries(ORB_SLAM
    ${catkin_LIBRARIES}
    ${OpenCV_LIBRARIES}
    ${Eigen_LIBRARIES}
    ${Boost_LIBRARIES}
    cholmod
    ${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_core.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_solver_cholmod.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_solver_dense.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_stuff.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_sba.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_sim3.so
    ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o_types_slam3d.so
    )

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS ORB_SLAM ORB_SLAM_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_ORB_SLAM.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
