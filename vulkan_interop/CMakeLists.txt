cmake_minimum_required(VERSION 3.18 FATAL_ERROR)
project(VulkanCudaInterop LANGUAGES CXX CUDA)

# Set C++ standard
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set CUDA standard
set(CMAKE_CUDA_STANDARD 17)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

# Enable CUDA language support
enable_language(CUDA)

# Find required packages
find_package(Vulkan REQUIRED)
find_package(glfw3 REQUIRED)

# Set CUDA architectures for RTX 2080 (Turing, compute capability 7.5)
set(CMAKE_CUDA_ARCHITECTURES 75)

# Include directories
include_directories(
    ${Vulkan_INCLUDE_DIRS}
    ${GLFW3_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
)

# Source files
set(SOURCES
    VulkanCudaInterop.cpp
    VulkanCudaInterop.cu
    Ex16_VulkanInterop.cu
)

# Create executable
add_executable(${PROJECT_NAME} ${SOURCES})

# Link libraries
target_link_libraries(${PROJECT_NAME}
    ${Vulkan_LIBRARIES}
    ${GLFW3_LIBRARIES}
    cuda
    cudart
)

# Compiler flags
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<COMPILE_LANGUAGE:CXX>:-Wall -Wextra>
)

# CUDA compiler flags
target_compile_options(${PROJECT_NAME} PRIVATE
    $<$<COMPILE_LANGUAGE:CUDA>:--expt-extended-lambda --expt-relaxed-constexpr>
)

# Define for Linux build
target_compile_definitions(${PROJECT_NAME} PRIVATE
    VK_USE_PLATFORM_XLIB_KHR
)
