cmake_minimum_required(VERSION 2.8)

project(tilemaker)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

if("${PROJECT_SOURCE_DIR}" STREQUAL "${PROJECT_BINARY_DIR}")
  message(FATAL_ERROR "In-source builds are not allowed, please use a separate build directory.")
endif()

include_directories(include)
include_directories(${CMAKE_BINARY_DIR}) # for generated files

find_package(Boost 1.56 REQUIRED COMPONENTS system filesystem program_options)
include_directories(SYSTEM ${Boost_INCLUDE_DIRS})

find_package(Protobuf REQUIRED)
include_directories(${PROTOBUF_INCLUDE_DIR})

find_package(SQLite3 REQUIRED)
include_directories(${SQLITE3_INCLUDE_DIR})

find_package(libshp REQUIRED)
include_directories(${LIBSHP_INCLUDE_DIR})

find_package(Lua REQUIRED)
include_directories(${LUA_INCLUDE_DIR})

find_package(Luabind REQUIRED)
include_directories(${LUABIND_INCLUDE_DIR})

find_package(ZLIB REQUIRED)
include_directories(${ZLIB_INCLUDE_DIR})

if(MSVC)
  add_definitions(-D_USE_MATH_DEFINES)
else()
  add_definitions(-std=c++11)
endif()

ADD_CUSTOM_COMMAND(OUTPUT vector_tile.pb.cc vector_tile.pb.h
                   COMMAND ${PROTOBUF_PROTOC_EXECUTABLE}
		   ARGS --cpp_out ${CMAKE_BINARY_DIR} -I ${CMAKE_SOURCE_DIR}/include ${CMAKE_SOURCE_DIR}/include/vector_tile.proto)

ADD_CUSTOM_COMMAND(OUTPUT osmformat.pb.cc osmformat.pb.h
                   COMMAND ${PROTOBUF_PROTOC_EXECUTABLE}
		   ARGS --cpp_out ${CMAKE_BINARY_DIR} -I ${CMAKE_SOURCE_DIR}/include ${CMAKE_SOURCE_DIR}/include/osmformat.proto)

add_executable(tilemaker vector_tile.pb.cc osmformat.pb.cc src/tilemaker.cpp)
target_link_libraries(tilemaker ${Boost_LIBRARIES} ${PROTOBUF_LIBRARY} ${LIBSHP_LIBRARIES} ${SQLITE3_LIBRARIES} ${LUABIND_LIBRARIES} ${LUA_LIBRARIES} ${ZLIB_LIBRARY})

install(TARGETS tilemaker RUNTIME DESTINATION bin)
