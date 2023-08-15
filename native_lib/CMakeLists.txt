cmake_minimum_required(VERSION 3.16)

project(stable_diffusion_library VERSION 0.0.1 LANGUAGES C CXX)
set(CMAKE_CXX_STANDARD 20)
 
message(STATUS "START COMPILE stable_diffusion")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC")

set(stable_diffusion_shared_dir "lib")
set(stable_diffusion_exe_dir "bin")
set(prefix_shared_library "lib")
 

if (ANDROID)
 set(package_shared_extension "_${ANDROID_ARCH_NAME}.so")
 set(package_exe_extension "")
 set(package_name "stable_diffusion_${ANDROID_ARCH_NAME}")
elseif (WIN32)
 set(package_shared_extension ".dll")
 set(package_exe_extension ".exe") 
 set(package_name "stable_diffusion")
elseif (APPLE)
 set(package_shared_extension ".dylib")
 set(package_exe_extension "") 
 set(package_name "stable_diffusion")
else()
 set(package_shared_extension ".so")
 set(package_exe_extension "") 
 set(package_name "stable_diffusion")
endif()


add_library(stable_diffusion_shared SHARED stable_diffusion.cpp)
add_executable(stable_diffusion_exe stable_diffusion.cpp)

set_target_properties(stable_diffusion_shared PROPERTIES 
  LIBRARY_OUTPUT_DIRECTORY "${stable_diffusion_shared_dir}"
  PREFIX "${prefix_shared_library}"
  OUTPUT_NAME "${package_name}"
)

set_target_properties(stable_diffusion_exe PROPERTIES 
  RUNTIME_OUTPUT_DIRECTORY "${stable_diffusion_exe_dir}"
  OUTPUT_NAME "${package_name}"
)

if (WIN32)
 set_target_properties(stable_diffusion_shared PROPERTIES
  WINDOWS_EXPORT_ALL_SYMBOLS ON
)
endif (WIN32)
 
add_custom_command(
  TARGET stable_diffusion_shared POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${stable_diffusion_shared_dir}/${prefix_shared_library}stable_diffusion${package_shared_extension}"
)

add_custom_command(
  TARGET stable_diffusion_exe POST_BUILD 
  COMMAND ${CMAKE_STRIP} --strip-debug --strip-unneeded "${stable_diffusion_exe_dir}/stable_diffusion${package_exe_extension}" 
)

 