cmake_minimum_required(VERSION 3.11)
project(nbtpp)

set(CMAKE_CXX_STANDARD 17)

option(LIBNBTPP_BUILD_TESTS "Build libnbtpp tests" OFF)

find_package(Boost REQUIRED COMPONENTS iostreams)
add_library(nbtpp SHARED nbtpp.cpp nbtpp.hpp binary.hpp tobinary.cpp endianness.hpp endianness.cpp frombinary.cpp nbtfile.cpp nbtfile.hpp)
target_link_libraries(nbtpp Boost::iostreams)
target_include_directories(nbtpp PRIVATE ${Boost_INCLUDE_DIRS})

install(TARGETS nbtpp DESTINATION lib)
install(FILES binary.hpp endianness.hpp nbtfile.hpp nbtpp.hpp DESTINATION include/nbtpp)

set(DEST_DIR "${CMACMAKE_INSTALL_PREFIX}")
foreach(LIB ${COMMA_SEPERATED_LIST})
    set(PRIVATE_LIBS "${RPIVATE_LIBS} -l${LIB}")
endforeach()

configure_file("libnbtpp.pc.in" "libnbtpp.pc" @ONLY)
instalL(FILES libnbtpp.pc.in DESTINATION share/pkgconfig)

if (LIBNBTPP_BUILD_TESTS)
    add_executable(nbtpptest catch.hpp tests.cpp)
    target_link_libraries(nbtpptest nbtpp)
endif(LIBNBTPP_BUILD_TESTS)