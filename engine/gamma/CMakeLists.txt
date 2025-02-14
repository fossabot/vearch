#--------------------------------------------
#    GAMMA ENGINE 
#--------------------------------------------
PROJECT(gamma_engine C CXX)
CMAKE_MINIMUM_REQUIRED(VERSION 3.0)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/Modules)

FIND_PACKAGE(Faiss REQUIRED)

OPTION(BUILD_TEST "Build tests" OFF)

#ENV VARs
SET(THIRDPARTY ${CMAKE_CURRENT_SOURCE_DIR}/third_party)
SET(EASYLOGGINGPP ${CMAKE_CURRENT_SOURCE_DIR}/third_party/easyloggingpp)
SET(HASHTABLE ${CMAKE_CURRENT_SOURCE_DIR}/third_party/libcuckoo)
SET(UTIL ${CMAKE_CURRENT_SOURCE_DIR}/util)
SET(GAMMA_SEARCH ${CMAKE_CURRENT_SOURCE_DIR}/search)
SET(GAMMA_INDEX ${CMAKE_CURRENT_SOURCE_DIR}/index)
SET(GAMMA_NUM_INDEX ${CMAKE_CURRENT_SOURCE_DIR}/index/numeric)
SET(GAMMA_VECTOR ${CMAKE_CURRENT_SOURCE_DIR}/vector)
SET(GAMMA_REALTIME ${CMAKE_CURRENT_SOURCE_DIR}/realtime)
SET(GAMMA_PROFILE ${CMAKE_CURRENT_SOURCE_DIR}/profile)
SET(GAMMA_C_API ${CMAKE_CURRENT_SOURCE_DIR}/c_api)


SET(CMAKE_CXX_FLAGS_DEBUG "$ENV{CXXFLAGS} -std=c++11 -mavx2 -msse4 -mpopcnt -fopenmp -D_FILE_OFFSET_BITS=64 -D_LARGE_FILE -DOPEN_CORE -O0 -w -g3 -gdwarf-2")
SET(CMAKE_CXX_FLAGS_RELEASE "$ENV{CXXFLAGS} -std=c++11 -fPIC -m64 -Wall -O3 -mavx2 -msse4 -mpopcnt -fopenmp -D_FILE_OFFSET_BITS=64 -D_LARGE_FILE -Werror=narrowing -Wno-deprecated")

#INCLUDE DIR
INCLUDE_DIRECTORIES(
    ${THIRDPARTY}
    ${EASYLOGGINGPP}
    ${HASHTABLE}
    ${UTIL}
    ${GAMMA_PROFILE}
    ${GAMMA_SEARCH}
    ${GAMMA_INDEX}
    ${GAMMA_NUM_INDEX}
    ${GAMMA_VECTOR}
    ${GAMMA_REALTIME}
    ${GAMMA_C_API}
    ${Faiss_INCLUDE_DIR}
)

#LINK LIBRARY
SET(LIBRARIES
    ${Faiss_LIB}
)

#ADDITIONAL SOURCE DIR
AUX_SOURCE_DIRECTORY(${THIRDPARTY} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${EASYLOGGINGPP} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${UTIL} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_SEARCH} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_INDEX} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_NUM_INDEX} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_VECTOR} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_REALTIME} DIR_SRCS)
AUX_SOURCE_DIRECTORY(${GAMMA_PROFILE} DIR_SRCS)

SET(LIB_SRCS ${DIR_SRCS})

# ADD DEFINITION
ADD_DEFINITIONS(-DHAVE_NETINET_IN_H)
ADD_DEFINITIONS(-DFINTEGER=int)

IF (CMAKE_BUILD_TYPE STREQUAL "Debug")
    MESSAGE(STATUS "Debug Mode")
    MESSAGE(STATUS "Flags:" ${CMAKE_CXX_FLAGS_DEBUG})
    ADD_DEFINITIONS(-DDEBUG_) 
ELSE (CMAKE_BUILD_TYPE STREQUAL "Debug")
    SET(CMAKE_BUILD_TYPE "Release" CACHE STRING "build type" FORCE)
    MESSAGE(STATUS "Release Mode")
    MESSAGE(STATUS "Flags:" ${CMAKE_CXX_FLAGS_RELEASE})
ENDIF (CMAKE_BUILD_TYPE STREQUAL "Debug")

IF (PERFORMANCE_TESTING STREQUAL "ON")
    MESSAGE(STATUS "With preformance testing")
    ADD_DEFINITIONS(-DPERFORMANCE_TESTING) 
ENDIF (PERFORMANCE_TESTING STREQUAL "ON")

ADD_LIBRARY(gamma SHARED ${LIB_SRCS})

TARGET_LINK_LIBRARIES(gamma ${LIBRARIES})

SET_TARGET_PROPERTIES(gamma PROPERTIES VERSION 0.1 SOVERSION 0.1)

INSTALL(TARGETS gamma
	LIBRARY	DESTINATION	lib 
	ARCHIVE	DESTINATION	lib)

INSTALL(FILES ${GAMMA_C_API}/gamma_api.h DESTINATION include)

IF(BUILD_TEST)
    ADD_SUBDIRECTORY(${CMAKE_CURRENT_SOURCE_DIR}/tests)
ENDIF(BUILD_TEST)
