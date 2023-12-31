cmake_minimum_required(VERSION 3.21)
project(OccEasyDrawing)
set(CMAKE_CXX_STANDARD 20)

add_compile_options("$<$<C_COMPILER_ID:MSVC>:/utf-8>")
add_compile_options("$<$<CXX_COMPILER_ID:MSVC>:/utf-8>")

find_package(OpenCASCADE CONFIG REQUIRED PATHS ${OpenCASCADE_DIR})
find_package(Eigen3 CONFIG REQUIRED)

set(SOURCES 
	"include/OccEasyDrawing/Common/Common.hpp"
	"include/OccEasyDrawing/Common/ShapeRenderStyle.hpp"
	"include/OccEasyDrawing/EasyDrawing.hpp"
	"src/EasyDrawing.cpp")

add_library(${PROJECT_NAME} STATIC ${SOURCES})
target_include_directories(${PROJECT_NAME} PUBLIC include)
target_link_libraries(${PROJECT_NAME} PUBLIC ${OpenCASCADE_LIBRARIES})
target_link_libraries(${PROJECT_NAME} PUBLIC Eigen3::Eigen)
if(USE_RELEASE_OPENCASCADE)
    install(DIRECTORY "${OpenCASCADE_BINARY_DIR}i/" TYPE BIN)
else ()
    install(DIRECTORY "${OpenCASCADE_BINARY_DIR}$<$<CONFIG:Debug>:d>$<$<CONFIG:Release>:i>$<$<CONFIG:RelWithDebInfo>:i>/" TYPE BIN)
endif ()

