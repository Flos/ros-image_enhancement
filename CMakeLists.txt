cmake_minimum_required(VERSION 2.8.3)
project(image_enhancement)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  dynamic_reconfigure
  image_transport
  nodelet
  roscpp
  rospy
  sensor_msgs
)

## System dependencies are found with CMake's conventions
find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()


#add dynamic reconfigure api
#find_package(catkin REQUIRED dynamic_reconfigure)
generate_dynamic_reconfigure_options(
  cfg/blur.cfg
  cfg/brightness.cfg
  cfg/clahe.cfg
  cfg/dilate.cfg
  cfg/erode.cfg
  cfg/gamma.cfg
)

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
  LIBRARIES image_enhancement
  CATKIN_DEPENDS OpenCV boost cv_bridge dynamic_reconfigure image_transport nodelet roscpp rospy sensor_msgs
  DEPENDS system_lib
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(include)
include_directories(
	${catkin_INCLUDE_DIRS}
	include
)


# Image enhancement Nodelets
add_library(brightness
	src/brightness.cpp
)
target_link_libraries(brightness
	${catkin_LIBRARIES}
)

add_dependencies(brightness 
	${PROJECT_NAME}_gencfg
)

add_library(clahe
	src/clahe.cpp
)
target_link_libraries(clahe
	${catkin_LIBRARIES}
)

add_dependencies(clahe 
	${PROJECT_NAME}_gencfg
)

add_library(gamma
	src/gamma.cpp
)
target_link_libraries(gamma
  	${catkin_LIBRARIES}
)

add_dependencies(gamma 
	${PROJECT_NAME}_gencfg
)

add_library(blur
	src/blur.cpp
)
target_link_libraries(blur
	${catkin_LIBRARIES}
)

add_dependencies(blur 
	${PROJECT_NAME}_gencfg
)

# Erode
add_library(erode
	src/erode.cpp
)

add_dependencies(erode 
	${PROJECT_NAME}_gencfg
)

target_link_libraries(erode
	${catkin_LIBRARIES}
)

# Diletate
add_library(dilate
	src/dilate.cpp
)

add_dependencies(dilate 
	${PROJECT_NAME}_gencfg
)

target_link_libraries(dilate
	${catkin_LIBRARIES}
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
# install(TARGETS image_enhancement image_enhancement_node
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
# catkin_add_gtest(${PROJECT_NAME}-test test/test_image_enhancement.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
