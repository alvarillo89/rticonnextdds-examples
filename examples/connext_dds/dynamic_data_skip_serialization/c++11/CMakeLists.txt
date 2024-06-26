#
# (c) 2024 Copyright, Real-Time Innovations, Inc.  All rights reserved.
#
# RTI grants Licensee a license to use, modify, compile, and create derivative
# works of the Software.  Licensee has the right to distribute object form
# only for use with RTI products.  The Software is provided "as is", with no
# warranty of any type, including any warranty for fitness for any purpose.
# RTI is under no obligation to maintain or support the Software.  RTI shall
# not be liable for any incidental or consequential damages arising out of the
# use or inability to use the software.
#
cmake_minimum_required(VERSION 3.11)
project(rtiexamples-dynamic-data-skip-serialization)
list(APPEND CMAKE_MODULE_PATH
    "${CMAKE_CURRENT_SOURCE_DIR}/../../../../resources/cmake/Modules"
)
include(ConnextDdsConfigureCmakeUtils)
connextdds_configure_cmake_utils()

# Find the RTI Connext DDS libraries
if(NOT RTIConnextDDS_FOUND)
    find_package(RTIConnextDDS
        "7.3.0"
        REQUIRED
        COMPONENTS
            core
    )
endif()

set(CMAKE_CXX_STANDARD 11)
set(PLATFORM_MODERN_CXX_STANDARD 11)

add_executable(recorder_cxx2
    "${CMAKE_CURRENT_SOURCE_DIR}/util.cxx"
    "${CMAKE_CURRENT_SOURCE_DIR}/recorder.cxx"
)

target_link_libraries(recorder_cxx2
    PUBLIC
        RTIConnextDDS::cpp2_api
)

target_include_directories(recorder_cxx2
    PRIVATE
        "${CMAKE_CURRENT_BINARY_DIR}/src"
)

set_target_properties(recorder_cxx2
    PROPERTIES
        OUTPUT_NAME "recorder")


if(CMAKE_SYSTEM_NAME MATCHES "Linux" AND CMAKE_CXX_COMPILER_ID MATCHES "GNU")
    set_target_properties(recorder_cxx2
        PROPERTIES
            LINK_FLAGS -Wl,--no-as-needed)
endif()
