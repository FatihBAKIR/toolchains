cmake_minimum_required(VERSION 3.2)

set(AVR_CPU atmega328p)
set(CMAKE_TOOLCHAIN_FILE "../../cmake/DockerToolchain.cmake")

project(contmake)

add_executable(lolz main.cpp test.hpp)
target_include_directories(lolz PUBLIC .)