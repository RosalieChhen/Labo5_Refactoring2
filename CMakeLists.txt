cmake_minimum_required(VERSION 3.12)
project(GEN_Refactor2)

set(CMAKE_CXX_STANDARD 14)

include(CTest)
include_directories(src)

if (CMAKE_VERSION VERSION_LESS 3.2)
    set(UPDATE_DISCONNECTED_IF_AVAILABLE "")
else()
    set(UPDATE_DISCONNECTED_IF_AVAILABLE "UPDATE_DISCONNECTED 1")
endif()

include(DownloadProject.cmake)
download_project(PROJ                googletest
                 GIT_REPOSITORY      https://github.com/google/googletest.git
                 GIT_TAG             master
                 ${UPDATE_DISCONNECTED_IF_AVAILABLE}
)

# Prevent GoogleTest from overriding our compiler/linker options
# when building with Visual Studio
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

add_subdirectory(${googletest_SOURCE_DIR} ${googletest_BINARY_DIR})

# When using CMake 2.8.11 or later, header path dependencies
# are automatically added to the gtest and gmock targets.
# For earlier CMake versions, we have to explicitly add the
# required directories to the header search path ourselves.
if (CMAKE_VERSION VERSION_LESS 2.8.11)
    include_directories("${gtest_SOURCE_DIR}/include"
                        "${gmock_SOURCE_DIR}/include")
endif()


add_executable(GEN_Refactor2
        src/Customer.cpp
        src/Customer.h
        #src/main.cpp
        src/Movie.cpp
        src/Movie.h
        src/Rental.h
        src/tests.cpp)
target_link_libraries(GEN_Refactor2 gtest_main gmock_main)
add_test(NAME test COMMAND GEN_Refactor2)









