cmake_minimum_required(VERSION 3.3)

get_filename_component(NAME ${CMAKE_SOURCE_DIR} NAME)
project(${NAME})

set(WIN_CPP_PREFIX_PATH $ENV{WIN_CPP_PREFIX_PATH})
if (NOT WIN_CPP_PREFIX_PATH)
    message("Cannot find WIN_CPP_PREFIX_PATH")
endif()
if (NOT EXISTS "${WIN_CPP_PREFIX_PATH}")
    message("${WIN_CPP_PREFIX_PATH} does not exist")
else()
    set(CMAKE_PREFIX_PATH "${WIN_CPP_PREFIX_PATH}")
    set(CMAKE_MODULE_PATH "${WIN_CPP_PREFIX_PATH}/cmake-modules")
    set(ZLIB_ROOT ${WIN_CPP_PREFIX_PATH})
endif()

if(CMAKE_COMPILER_IS_GNUCXX)
   SET(ENABLE_CXX11 "-std=c++0x")

   EXECUTE_PROCESS(COMMAND "${CMAKE_CXX_COMPILER} -dumpversion" OUTPUT_VARIABLE GCC_VERSION)
   if (GCC_VERSION VERSION_LESS 4.7)
      SET(ENABLE_CXX11 "-std=c++0x")
   endif()

   SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${ENABLE_CXX11}")
endif()

find_package(OpenGL REQUIRED)

find_package(GLM REQUIRED)
include_directories(${GLM_INCLUDE_DIR})

include(files.cmake)

include_directories(${CMAKE_SOURCE_DIR}/include)
include_directories(${CMAKE_SOURCE_DIR}/../deps/include)

add_executable(${NAME} ${FILES})

target_link_libraries(${NAME}
    ${OPENGL_LIBRARIES}
)
