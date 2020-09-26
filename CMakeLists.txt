cmake_minimum_required(VERSION 2.8.9)
set(COMPONENT_NAME actkbd)  #set the name of the binary

project(${COMPONENT_NAME})
set(AUTO_SVN_VERSION ON CACHE BOOL ON) #by default include svn revision numbers
file(GLOB SOURCES "*.c")

# Add #include path of all dependent components
include_directories(${CMAKE_CURRENT_BINARY_DIR})
add_executable(${COMPONENT_NAME} ${SOURCES})
#add_definitions(-DACTKBDVERSION={"00.01"})

install(TARGETS ${COMPONENT_NAME} DESTINATION sbin)

#automatically include svn revision to a headerfile which will be included in this component
if(${AUTO_SVN_VERSION})
	find_package(Subversion REQUIRED)
	if(SUBVERSION_FOUND)
		Subversion_WC_INFO(${PROJECT_SOURCE_DIR} Project)
		FILE(WRITE ${CMAKE_CURRENT_BINARY_DIR}/SrcControlVersion.h "#define SRC_CONTROL_VERSION ${Project_WC_LAST_CHANGED_REV}\n")
  	else()
		message( FATAL_ERROR "unable to read sub-version Revision number, this is needed for embedding into this component" )
	endif()
else()
	FILE(WRITE ${CMAKE_CURRENT_BINARY_DIR}/SrcControlVersion.h "#define SRC_CONTROL_VERSION 0001\n")
endif()
