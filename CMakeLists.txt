cmake_minimum_required(VERSION 2.8)

project(my_project)

include_directories(${CMAKE_SOURCE_DIR}/inc/)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin/)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib/)

set(CMAKE_C_COMPILER gcc)
set(CMAKE_CXX_COMPILER g++)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} \
  -std=c++11 \
  ")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} \
  -DSTANDALONE \
  -D__STDC_CONSTANT_MACROS \
  -D__STDC_LIMIT_MACROS \
  -DTARGET_POSIX \
  -D_LINUX \
  -fPIC \
  -DPIC \
  -D_REENTRANT \
  -D_LARGEFILE64_SOURCE \
  -D_FILE_OFFSET_BITS=64 \
  -U_FORTIFY_SOURCE \
  -Wall \
  -g \
  -DHAVE_LIBOPENMAX=2 \
  -DOMX \
  -DOMX_SKIP64BIT \
  -ftree-vectorize \
  -pipe \
  -DUSE_EXTERNAL_OMX \
  -DHAVE_LIBBCM_HOST \
  -DUSE_EXTERNAL_LIBBCM_HOST \
  -DUSE_VCHIQ_ARM \
  -Wno-psabi \
  ")

set(CMAKE_BUILD_TYPE Release)

#Add external include directories
include_directories(
  /usr/local/include/
  /opt/vc/include/
  /opt/vc/include/interface/vcos/pthreads/
  /opt/vc/include/interface/vmcs_host/linux/
  /opt/vc/src/hello_pi/
  /opt/vc/src/hello_pi/libs/ilclient/
  /opt/vc/src/hello_pi/libs/vgfont/
  )

#Link external libraries diectories
link_directories(
  /opt/vc/lib/
  /opt/vc/src/hello_pi/libs/ilclient/
  /opt/vc/src/hello_pi/libs/vgfont/
  )

#Link external libraries
link_libraries(
  -lbrcmGLESv2
  -lbrcmEGL
  -lopenmaxil
  -lbcm_host
  -lvcos
  -lvchiq_arm
  -lpthread
  -lrt
  -lm
  -lilclient
  -lvgfont
  )

#Add project source codes to project
set(SRC "" CACHE INTERNAL "")
file(GLOB SRC
  src/*.c
  src/*.cpp
  )

#Create project library
if(NOT "${SRC}" STREQUAL "")
  #Create the library
  message("[CORERAIN] Creating project library")
  add_library(${PROJECT_NAME} SHARED ${SRC})
elseif("${SRC}" STREQUAL "")
  message("[CORERAIN] No internal dependencies")
endif()

#Add sub-directories of the project
add_subdirectory(test/)
