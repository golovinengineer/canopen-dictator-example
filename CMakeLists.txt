cmake_minimum_required(VERSION 3.20...4.5)
project(firmware LANGUAGES C)

# The CanFestival stack the generated C is written for. Only its headers are
# used: include/ has no CMakeLists.txt, so the stack itself is not built.
include(FetchContent)
FetchContent_Declare(co_stack
  URL https://github.com/RoboticsHardwareSolutions/co_stack/archive/0e1892cbd9dd17ce34a164c58b4c59458bface79.tar.gz
  URL_HASH SHA256=cd31212b9acef34e9fac10d7dc663c00ca430a9fbce45d1a0f85e51adcf16552
  SOURCE_SUBDIR include)
FetchContent_MakeAvailable(co_stack)
add_library(canfestival INTERFACE)
target_include_directories(canfestival INTERFACE "${co_stack_SOURCE_DIR}/include")

find_package(Dictator 0.2 REQUIRED)
dictator_add_object_dictionary(od
  PROJECT  device.objdict.toml
  STACK    canfestival
  NAME     ObjDict
  LINK     canfestival
  EDS      device.eds
  MARKDOWN device.md)

# Compiled, not linked: the object dictionary refers to the stack's
# functions, which this example doesn't build.
add_library(firmware OBJECT main.c)
target_link_libraries(firmware PRIVATE od)
