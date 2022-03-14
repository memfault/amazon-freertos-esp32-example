cmake_minimum_required(VERSION 3.13)

set(PROJECT_NAME memfault-esp32-demo-app)
set(CMAKE_PROJECT_NAME ${PROJECT_NAME})

project(${PROJECT_NAME})

# Include the Memfault SDK
set(MEMFAULT_FIRMWARE_SDK components/memfault-firmware-sdk)
set(MEMFAULT_PLATFORM_PORT_COMPONENTS main)
# # disable Memfault HTTP client, conflicts with the AFR one
# set(MEMFAULT_ESP_HTTP_CLIENT_ENABLE 0)
include(${MEMFAULT_FIRMWARE_SDK}/ports/esp_idf/memfault.cmake)

# Pull in ESP-IDF defined CMake build infrastructure logic to configure the project, discover all the components, etc
set(esp_idf_dir "${CMAKE_CURRENT_LIST_DIR}/freertos/vendors/espressif/esp-idf")
include(${esp_idf_dir}/tools/cmake/idf.cmake)
get_filename_component(
    ABS_EXTRA_COMPONENT_DIRS
    "${MEMFAULT_FIRMWARE_SDK}/ports/esp_idf/memfault" ABSOLUTE
)
idf_build_component(${ABS_EXTRA_COMPONENT_DIRS})

# Need to set these to prevent AFR from injecting the demo ones >_<
set(IDF_PROJECT_EXECUTABLE ${PROJECT_NAME}.elf)
set(IDF_EXECUTABLE_SRCS
  # stop AFR from swapping in their own exe srcs
  ${CMAKE_CURRENT_LIST_DIR}/main/stub.c
)

# Add our sources as a "main" component
get_filename_component(
    ABS_EXTRA_COMPONENT_DIRS
    "${CMAKE_CURRENT_LIST_DIR}/main/" ABSOLUTE
)
idf_build_component(${ABS_EXTRA_COMPONENT_DIRS})

# Inject a custom FreeRTOSConfig.h
include_directories(BEFORE freertos_configs)

# Add FreeRTOS as a subdirectory. AFR_BOARD tells which board to target.
set(AFR_BOARD espressif.esp32_wrover_kit CACHE INTERNAL "")
add_subdirectory(freertos)

# Include our "main" component when linking
target_link_libraries(${IDF_PROJECT_EXECUTABLE}
  PRIVATE
  idf::main
)

# # Add the Memfault Build ID so each build can have a unique version.
# add_custom_command(TARGET ${IDF_PROJECT_EXECUTABLE}
#   POST_BUILD
#   COMMAND python ${MEMFAULT_FIRMWARE_SDK}/scripts/fw_build_id.py ${IDF_PROJECT_EXECUTABLE})
