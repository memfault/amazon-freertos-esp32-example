cmake_minimum_required(VERSION 3.13)

set(PROJECT_NAME memfault-esp32-demo-app)
set(CMAKE_PROJECT_NAME ${PROJECT_NAME})

project(${PROJECT_NAME})

set(MEMFAULT_FIRMWARE_SDK components/memfault-firmware-sdk)
set(MEMFAULT_PLATFORM_PORT_COMPONENTS main)
include(${MEMFAULT_FIRMWARE_SDK}/ports/esp_idf/memfault.cmake)

set(esp_idf_dir "${CMAKE_CURRENT_LIST_DIR}/freertos/vendors/espressif/esp-idf")
# Pull in ESP-IDF defined CMake build infrastructure logic to configure the project, discover all the components, etc
include(${esp_idf_dir}/tools/cmake/idf.cmake)

# Need to set these to prevent AFR from injecting the demo ones >_<
set(IDF_PROJECT_EXECUTABLE ${PROJECT_NAME}.elf)
get_filename_component(TOP_DIR
  "${CMAKE_CURRENT_LIST_DIR}/main"
  ABSOLUTE
)
set(IDF_EXECUTABLE_SRCS
  # ${TOP_DIR}/cmd_system.c
  # ${TOP_DIR}/cmd_wifi.c
  # ${TOP_DIR}/config.c
  # ${TOP_DIR}/console_example_main.c
  # ${TOP_DIR}/memfault_platform_device_info.c
  # keep this around for now... in case we remove the above, still need to block
  # AFR from swapping in their own exe srcs
  ${TOP_DIR}/stub.c
)

# Inject a custom FreeRTOSConfig.h
include_directories(BEFORE freertos_configs)

# Add FreeRTOS as a subdirectory. AFR_BOARD tells which board to target.
set(AFR_BOARD espressif.esp32_wrover_kit CACHE INTERNAL "")
add_subdirectory(freertos)
add_subdirectory(main)

target_link_libraries(${PROJECT_NAME}.elf PRIVATE
)

# # Add the Memfault Build ID so each build can have a unique version.
# add_custom_command(TARGET ${IDF_PROJECT_EXECUTABLE}
#   POST_BUILD
#   COMMAND python ${MEMFAULT_FIRMWARE_SDK}/scripts/fw_build_id.py ${IDF_PROJECT_EXECUTABLE})
