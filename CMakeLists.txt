cmake_minimum_required(VERSION 3.3.2)
project(ndsloader CXX)

# build type
if (NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE Release)
endif ()

if (CMAKE_BUILD_TYPE MATCHES Debug)
    set(PROJECT_CXXFLAGS "${PROJECT_CXXFLAGS} -O0")
    set(CMAKE_VERBOSE_MAKEFILE TRUE)
else ()
    set(PROJECT_CXXFLAGS "${PROJECT_CXXFLAGS} -O3")
endif ()

message("Build type: ${CMAKE_BUILD_TYPE}")

# project files
set(PROJECT_SOURCEFILES

	nds.h
	nds.cpp
)

# external cmake modules
set(CMAKE_MODULE_PATH

    ${CMAKE_MODULE_PATH}
    ${CMAKE_SOURCE_DIR}/cmake_modules
)

# ida sdk 
find_package(IDASDK REQUIRED)

set(PROJECT_INCLUDEDIRECTORIES

	${PROJECT_INCLUDEDIRECTORIES}
	${IDASDK_INCLUDEDIRECTORIES}
)

set(PROJECT_DEFINITIONS ${PROJECT_DEFINITIONS} ${IDASDK_DEFINITIONS})
set(PROJECT_LIBRARIES ${PROJECT_LIBRARIES} ${IDASDK_LIBRARIES})

# output directories
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# static files
file(COPY ${CMAKE_SOURCE_DIR}/README.md DESTINATION ${CMAKE_BINARY_DIR}/lib)

# project settings
set(PROJECT_OUTPUTFILENAME "nds")

set(CMAKE_CXX_FLAGS ${PROJECT_CXXFLAGS})
add_definitions(${PROJECT_DEFINITIONS})

include_directories(${PROJECT_INCLUDEDIRECTORIES})
link_directories(${PROJECT_LIBRARYFOLDERPATHLIST})

add_library(${PROJECT_OUTPUTFILENAME} SHARED ${PROJECT_SOURCEFILES})
target_link_libraries(${PROJECT_OUTPUTFILENAME} ${PROJECT_LIBRARIES})

set_target_properties(${PROJECT_OUTPUTFILENAME} PROPERTIES COMPILE_FLAGS "-m32" LINK_FLAGS "-m32")
set_target_properties(${PROJECT_OUTPUTFILENAME} PROPERTIES PREFIX "")
set_target_properties(${PROJECT_OUTPUTFILENAME} PROPERTIES SUFFIX ".llx")
