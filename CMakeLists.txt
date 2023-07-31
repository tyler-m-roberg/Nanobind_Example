cmake_minimum_required(VERSION 3.15...3.26)
#project(${SKBUILD_PROJECT_NAME} LANGUAGES CXX)
project(${SKBUILD_PROJECT_NAME} LANGUAGES CXX C)

set(Project_Base_DIR "C:\\Users\\theoz\\source\\repos\\NanoBindExample")

set(nanobind_DIR "${Project_Base_DIR}\\.venv\\Lib\\site-packages\\nanobind\\cmake")
set(Arrow_DIR "${Project_Base_DIR}\\out\\build\\x64-debug\\vcpkg_installed\\x64-windows\\share\\arrow")

find_package(Python 3.8 REQUIRED COMPONENTS Interpreter Development.Module)

find_package(nanobind CONFIG REQUIRED)

find_package(Arrow CONFIG REQUIRED)

message("---Linking DWDataReader")
add_library(DWDataReaderLib64 MODULE DWDataReaderLib64.dll)
set_target_properties(DWDataReaderLib64 PROPERTIES LINKER_LANGUAGE C)

nanobind_add_module(nanobind_example NB_STATIC src/nanobind_example/example.cpp src/nanobind_example/DWLoadLib.c)

target_include_directories(
    nanobind_example
    PUBLIC
    "${Project_Base_DIR}/.venv/Lib/site-packages/nanobind/include"
    "${Project_Base_DIR}/.venv/Lib/site-packages/pyarrow/include"
    C:/Users/theoz/AppData/Local/Programs/Python/Python311/include
)

target_link_directories(nanobind_example PUBLIC "${Project_Base_DIR}/.venv/Lib/site-packages/pyarrow")
target_link_libraries(nanobind_example PUBLIC Arrow::arrow_shared arrow_python)

install(TARGETS nanobind_example LIBRARY DESTINATION ./nanobind_example)

