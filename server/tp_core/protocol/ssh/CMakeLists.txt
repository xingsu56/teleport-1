cmake_minimum_required(VERSION 3.5)

MESSAGE(STATUS "=======================================================")
MESSAGE(STATUS "               libtpssh")
MESSAGE(STATUS "=======================================================")
#MESSAGE(STATUS "operation system is ${CMAKE_SYSTEM}")
#MESSAGE(STATUS "current source directory is ${CMAKE_CURRENT_SOURCE_DIR}")
include(../../../../CMakeCfg.txt)


set(CMAKE_CXX_FLAGS "-fPIC")
set(CMAKE_C_FLAGS "-fPIC")

aux_source_directory(. DIR_SSH_SRCS)
aux_source_directory(../../common DIR_SSH_SRCS)
aux_source_directory(../../../../common/libex/src DIR_SSH_SRCS)
aux_source_directory(../../../../external/jsoncpp/src/lib_json DIR_SSH_SRCS)

list(REMOVE_ITEM DIR_SSH_SRCS "./dllmain.cpp")
list(REMOVE_ITEM DIR_SSH_SRCS "./stdafx.cpp")


include_directories(
		../../../../common/libex/include
		../../../../common/teleport
		../../../../external/jsoncpp/include
)

include_directories(
		${TP_EXTERNAL_RELEASE_DIR}/include
)
link_directories(${TP_EXTERNAL_RELEASE_DIR}/lib)


add_library(tpssh SHARED ${DIR_SSH_SRCS})

if (OS_LINUX)
	target_link_libraries(tpssh ssh ssl crypto mbedx509 mbedtls mbedcrypto dl pthread rt util)
elseif (OS_MACOS)
	target_link_libraries(tpssh ssh ssl crypto mbedx509 mbedtls mbedcrypto dl pthread util)
endif()
