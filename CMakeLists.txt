cmake_minimum_required(VERSION 3.10)

project(ArrowExamples)

set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake_modules" ${CMAKE_MODULE_PATH})

# This ensures that things like gnu++11 get passed correctly
if(NOT DEFINED CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 11)
endif()

# We require a C++11 compliant compiler
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Look for installed packages the system
if (NOT DEFINED OPENSSL_ROOT_DIR AND DEFINED APPLE)
  set(OPENSSL_ROOT_DIR /usr/local/Cellar/openssl@1.1/1.1.1m)
endif()
find_package(OpenSSL REQUIRED)
find_package(Arrow REQUIRED)
find_package(Parquet REQUIRED)

include_directories(${ARROW_INCLUDE_DIR} ${PARQUET_INCLUDE_DIR})

add_executable(compute_and_write_csv_example src/compute_and_write_csv_example.cc ${ARROW_LIBRARIES})
target_link_libraries(compute_and_write_csv_example ${ARROW_SHARED_LIB})

add_executable(reader_writer src/reader_writer.cc)
target_link_libraries(reader_writer ${ARROW_SHARED_LIB} ${PARQUET_SHARED_LIB})
