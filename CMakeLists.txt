cmake_minimum_required(VERSION 2.8.12)

set(ASMJIT_STATIC TRUE)
#Prevent errors Vulkan latest Cmake need FindXCB.cmake
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/rpcs3/cmake_modules/")

if (NOT CMAKE_BUILD_TYPE)
	message(STATUS "No build type selected, default to Release")
	set(CMAKE_BUILD_TYPE "Release")
endif()

if(NOT CMAKE_SIZEOF_VOID_P EQUAL 8)
	message( FATAL_ERROR "RPCS3 can only be compiled on 64-bit platforms." )
endif()

add_definitions(-DCMAKE_BUILD)

if(WIN32)
	# We use libpng's static library and don't need to build the shared library and run the tests
	set(PNG_SHARED OFF CACHE BOOL "Build shared lib." FORCE)
	set(PNG_TESTS  OFF CACHE BOOL "Build tests." FORCE)
	#Use system-supplied libpng on non-Windows platforms
	add_subdirectory( 3rdparty/libpng )
endif()
# TODO: do real installation, including copying directory structure
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE "${PROJECT_BINARY_DIR}/bin")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG "${PROJECT_BINARY_DIR}/bin")
add_subdirectory( Vulkan )
add_subdirectory( rpcs3 )
add_subdirectory( rsx_program_decompiler )
