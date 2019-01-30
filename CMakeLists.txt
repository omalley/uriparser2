# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required (VERSION 2.8)
cmake_policy(SET CMP0042 NEW) # suppress warning about mac rpath
cmake_policy(SET CMP0048 NEW) # allow setting project version

project(uriparser2
  VERSION "1.0.0"
  DESCRIPTION "Your one-stop C and C++ library for URI parsing."
  HOMEPAGE_URL "https://github.com/bnoordhuis/uriparser2"
  LANGUAGES C)

option(BUILD_SHARED_LIBRARIES
    "Should we build shared libraries instead of static ones?"
    ON)

enable_testing()

set (CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -O2")
list (APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

if (BUILD_SHARED_LIBRARIES)
  set (LIB_TYPE SHARED)
else ()
  set (LIB_TYPE STATIC)
endif (BUILD_SHARED_LIBRARIES)

find_package(Uriparser REQUIRED)

set (SOURCE_FILES uriparser2.c)

add_library(uriparser2 ${LIB_TYPE} ${SOURCE_FILES})
set_target_properties(uriparser2 PROPERTIES
  SOVERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}")
target_link_libraries(uriparser2 uriparser)

add_executable(uriparser2-test test-uriparser2.c)
target_link_libraries(uriparser2-test uriparser2)
add_test(NAME uriparser2-test COMMAND uriparser2-test)

install(FILES uriparser2.h DESTINATION include)
install(TARGETS uriparser2 DESTINATION lib)

