cmake_minimum_required(VERSION 3.10)
project(CasinoNumberGuessingGame)

set(CMAKE_CXX_STANDARD 14)
set(EXECUTABLE_NAME casino_game)
set(TESTS_NAME test_game)

find_package(ZLIB REQUIRED)

include_directories(src)

add_executable(${EXECUTABLE_NAME} src/main.cpp src/game.cpp src/player.cpp)

add_executable(${TESTS_NAME} tests/test_game.cpp src/game.cpp src/player.cpp)

target_link_libraries(${EXECUTABLE_NAME} PRIVATE ZLIB::ZLIB)
target_link_libraries(${TESTS_NAME} PRIVATE ZLIB::ZLIB)
