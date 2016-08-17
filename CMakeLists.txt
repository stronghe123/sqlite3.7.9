cmake_minimum_required(VERSION 3.5)
project(sqlite3)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99 -DSQLITE_CORE=1")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}) #like gcc: -I
file(GLOB sources "*.c")
file(GLOB headers "*.h")
add_executable(sqlite3 ${sources} ${headers})