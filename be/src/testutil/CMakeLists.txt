# Copyright 2012 Cloudera Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.


cmake_minimum_required(VERSION 2.6)

# where to put generated libraries
set(LIBRARY_OUTPUT_PATH "${BUILD_OUTPUT_ROOT_DIRECTORY}/testutil")

# where to put generated binaries
set(EXECUTABLE_OUTPUT_PATH "${BUILD_OUTPUT_ROOT_DIRECTORY}/testutil")

add_library(TestUtil
  impalad-query-executor.cc
  in-process-servers.cc
  desc-tbl-builder.cc
  test-udas.cc
  test-udfs.cc
)
add_dependencies(TestUtil thrift-deps)

add_library(TestUdfs SHARED test-udfs.cc)
add_dependencies(TestUdfs thrift-deps)

COMPILE_TO_IR(test-udfs.cc)
add_dependencies(test-udfs-ir thrift-deps)

add_library(TestUdas SHARED test-udas.cc)
add_dependencies(TestUdas thrift-deps)

add_executable(mini-impala-cluster
  mini-impala-cluster.cc
)

target_link_libraries(mini-impala-cluster ${IMPALA_LINK_LIBS})
