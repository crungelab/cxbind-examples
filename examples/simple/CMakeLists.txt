cmake_minimum_required(VERSION 3.15...3.26)

project(cxb_simple VERSION 0.1.0)

find_package(Python COMPONENTS Interpreter Development)
add_subdirectory(../../depot/pybind11 pybind11)

pybind11_add_module(_core MODULE src/generated.cpp)
cxbind_module(_core)

#Moved to cxbind_module
#execute_process(
#  COMMAND ${Python_EXECUTABLE} -c "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
#  OUTPUT_VARIABLE PYTHON_SITE_PACKAGES
#  OUTPUT_STRIP_TRAILING_WHITESPACE
#)
#message(STATUS "Python site packages directory: ${PYTHON_SITE_PACKAGES}")
#target_include_directories(_core PRIVATE "${PYTHON_SITE_PACKAGES}/cxbind/include")

target_sources(_core PRIVATE src/init.cpp)
target_compile_definitions(_core PRIVATE VERSION_INFO=${PROJECT_VERSION})

target_link_directories(_core PRIVATE ${Python_LIBRARY_DIRS})
set_target_properties(_core PROPERTIES OUTPUT_NAME_DEBUG "_core_d")


install(TARGETS _core DESTINATION cxb_simple)
