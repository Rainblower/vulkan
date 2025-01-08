cmake_minimum_required(VERSION 3.15)
project(vulkan)

set(CMAKE_CXX_STANDARD 17)

# Укажите путь к Vulkan SDK
set(VULKAN_SDK "/Users/evgenijkolesnikov/VulkanSDK/1.3.290.0/macOS")
include_directories(${VULKAN_SDK}/include)

# Укажите библиотеку Vulkan
link_directories(${VULKAN_SDK}/lib)

find_package(glfw3 REQUIRED)

set(GLFW_SDK "/opt/homebrew/Cellar/glfw/3.4")
include_directories(${GLFW_SDK}/include)

add_executable(vulkan main.cpp)

target_link_libraries(vulkan
        glfw
        vulkan.1
)

set(SHADERS_SOURCE_DIR ${CMAKE_SOURCE_DIR}/shaders)

set(SHADERS_OUTPUT_DIR ${CMAKE_BINARY_DIR}/shaders)

# Копируем только файлы с расширением .spv
file(GLOB_RECURSE SHADER_FILES "${SHADERS_SOURCE_DIR}/*.spv")

# Настройка команды копирования
add_custom_command(
        TARGET ${PROJECT_NAME} POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E make_directory ${SHADERS_OUTPUT_DIR}
        COMMAND ${CMAKE_COMMAND} -E copy ${SHADER_FILES} ${SHADERS_OUTPUT_DIR}
)

