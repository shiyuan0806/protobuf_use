cmake_minimum_required(VERSION 3.5)

# Set the project name
project (protobuf_example)
add_compile_options(-std=c++11 -w)
message(${CMAKE_CURRENT_SOURCE_DIR})
# find the protobuf compiler and libraries
set(Protobuf_ROOT ${CMAKE_CURRENT_SOURCE_DIR}/third_party/protobuf)
Set(Protobuf_DIR  ${Protobuf_ROOT}/cmake)
set(protobuf_MODULE_COMPATIBLE ON CACHE BOOL "")
#set(Protobuf_INCLUDE_DIRS "/media/shiyuan/Code/MNN/protobuf-3.11.2/protobuf/include")
find_package(Protobuf REQUIRED PATHS ${Protobuf_DIR})#
# check if protobuf was found
if(PROTOBUF_FOUND)
    message ("protobuf found")
else()
    message (FATAL_ERROR "Cannot find Protobuf")
endif()
  message(${Protobuf_PROTOC_EXECUTABLE})
  message(${Protobuf_INCLUDE_DIRS})
  message(${PROTOBUF_LIBRARIES})
# Generate the .h and .cxx files
protobuf_generate_cpp(PROTO_SRCS PROTO_HDRS AddressBook.proto)

# Print path to generated files
message ("PROTO_SRCS = ${PROTO_SRCS}")
message ("PROTO_HDRS = ${PROTO_HDRS}")

# Add an executable
add_executable(protobuf_example
    main.cpp
    ${PROTO_SRCS}
    ${PROTO_HDRS})

target_include_directories(protobuf_example
    PUBLIC
    ${PROTOBUF_INCLUDE_DIRS}
    ${CMAKE_CURRENT_BINARY_DIR}
)

# link the exe against the libraries
target_link_libraries(protobuf_example
    PUBLIC
    ${PROTOBUF_LIBRARIES}
)
