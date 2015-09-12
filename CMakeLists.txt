cmake_minimum_required(VERSION 3.0)

project(tivaware VERSION 2.1.1.71)

set(CMAKE_CROSSCOMPILING TRUE)

set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_PROCESSOR cortex-m4)

set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)

set(CMAKE_FIND_ROOT_PATH ${CMAKE_INSTALL_PREFIX})

set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mthumb")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -march=armv7e-m")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mtune=cortex-m4")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mcpu=cortex-m4")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfpu=fpv4-sp-d16")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfloat-abi=softfp")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -ffunction-sections")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fdata-sections")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -pedantic")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -c")

set(CMAKE_C_FLAGS_DEBUG "-g -D DEBUG -O0")
set(CMAKE_C_FLAGS_RELEASE "-Os")
set(CMAKE_C_FLAGS_MINSIZEREL "-Os")
set(CMAKE_C_FLAGS_RELWITHDEBINFO "-g -D DEBUG -Os")

set(CMAKE_BUILD_TYPE Release)

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON)

file(GLOB CommonInc RELATIVE ${PROJECT_SOURCE_DIR} inc/*.h)
file(GLOB DriverLibSrc RELATIVE ${PROJECT_SOURCE_DIR} driverlib/*.c)
file(GLOB DriverLibInc RELATIVE ${PROJECT_SOURCE_DIR} driverlib/*.h)

add_library(driver STATIC ${DriverLibSrc})

install(TARGETS driver EXPORT TivaWareDriver
        ARCHIVE DESTINATION lib/${PROJECT_NAME}
        INCLUDES DESTINATION include/${PROJECT_NAME}
        PUBLIC_HEADER DESTINATION include/${PROJECT_NAME})
install(FILES ${CommonInc} DESTINATION include/${PROJECT_NAME}/inc)
install(FILES ${DriverLibInc} DESTINATION include/${PROJECT_NAME}/driverlib)
install(EXPORT TivaWareDriver
        FILE ${PROJECT_NAME}-driver-config.cmake
        DESTINATION lib/${PROJECT_NAME})
