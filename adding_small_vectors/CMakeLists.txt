cmake_minimum_required(VERSION 3.10)
project(device_info LANGUAGES CXX CUDA)

# Find CUDA Toolkit
find_package(CUDAToolkit REQUIRED)

# Set CUDA standard
set(CMAKE_CUDA_STANDARD 14)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

# Set CUDA architectures (compute capabilities)
# Common architectures: 50 (Maxwell), 60 (Pascal), 70 (Volta), 75 (Turing), 80 (Ampere), 86 (Ampere), 89 (Ada Lovelace)
# For broader compatibility, target multiple architectures
set(CMAKE_CUDA_ARCHITECTURES 50 60 70 75 80 86 89)

# Add executable
add_executable(adding_vectors adding_vectors.cu)

# Link CUDA runtime library
target_link_libraries(adding_vectors CUDA::cudart)

# Set compile options for CUDA
target_compile_options(adding_vectors PRIVATE
    $<$<COMPILE_LANGUAGE:CUDA>:-lineinfo>
)
