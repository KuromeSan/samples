cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{DOLCESDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{DOLCESDK}/share/dolce.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define DOLCESDK to point to your SDK path!")
  endif()
endif()

project(touch_sample)
include("${DOLCESDK}/share/dolce.cmake" REQUIRED)

set(VITA_APP_NAME "Touch example")
set(VITA_TITLEID  "VSDK00019")
set(VITA_VERSION  "01.00")

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=gnu11")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

include_directories(
  ../common
)

add_executable(${PROJECT_NAME}
  src/main.c
  ../common/debugScreen.c
)

target_link_libraries(${PROJECT_NAME}
  SceDisplay_stub
  SceTouch_stub
)

dolce_create_self(${PROJECT_NAME}.self ${PROJECT_NAME})
dolce_create_vpk(${PROJECT_NAME}.vpk ${VITA_TITLEID} ${PROJECT_NAME}.self
  VERSION ${VITA_VERSION}
  NAME ${VITA_APP_NAME}
)
