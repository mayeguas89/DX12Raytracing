cmake_minimum_required(VERSION 3.0.0)

project(D3D12HelloWorld VERSION 0.1.0)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/DirectXShaderCompiler/cmake/modules")

find_package(D3D12 REQUIRED)

add_compile_options(-D_UNICODE)

set(PROJECT_FOLDER HelloTriangle)

file(GLOB cpu_source_files "${CMAKE_SOURCE_DIR}/${PROJECT_FOLDER}/*.cpp")
file(GLOB dxr_helpers_source_files "${CMAKE_SOURCE_DIR}/${PROJECT_FOLDER}/DXRHelpers/nv_helpers_dx12*.cpp")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/$<CONFIG> CACHE STRING "")

add_executable(${PROJECT_FOLDER} WIN32 ${cpu_source_files} ${dxr_helpers_source_files})
target_include_directories(${PROJECT_FOLDER} PUBLIC ${PROJECT_FOLDER}/ ${D3D12_INCLUDE_DIRS} DXRHelpers DXRHelpers/nv_helpers_dx12)
target_link_libraries(${PROJECT_FOLDER} PUBLIC ${D3D12_LIBRARIES} dxguid.lib)

add_custom_command(TARGET ${PROJECT_FOLDER} POST_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_FOLDER}/bin/x64/$<CONFIG>/shaders.hlsl ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
)
