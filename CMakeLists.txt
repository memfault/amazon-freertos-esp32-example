cmake_minimum_required(VERSION 3.13)

set(PROJECT_NAME memfault-esp32-demo-app)
set(CMAKE_PROJECT_NAME ${PROJECT_NAME})

set(MEMFAULT_FIRMWARE_SDK components/memfault-firmware-sdk)
set(MEMFAULT_PLATFORM_PORT_COMPONENTS src)
include(${MEMFAULT_FIRMWARE_SDK}/ports/esp_idf/memfault.cmake)

include($ENV{IDF_PATH}/tools/cmake/project.cmake)

project(${PROJECT_NAME})

# Tell IDF build to link against this target.
set(IDF_PROJECT_EXECUTABLE ${PROJECT_NAME}.elf)

# Add FreeRTOS as a subdirectory. AFR_BOARD tells which board to target.
set(AFR_BOARD espressif.esp32_wrover_kit CACHE INTERNAL "")
add_subdirectory(freertos)

# # Link against the mqtt library so that we can use it. Dependencies are transitively
# # linked.
# target_link_libraries(my_app PRIVATE AFR::core_mqtt)

add_custom_command(TARGET ${CMAKE_PROJECT_NAME}.elf
  POST_BUILD
  COMMAND
  python ${MEMFAULT_FIRMWARE_SDK}/scripts/fw_build_id.py ${CMAKE_PROJECT_NAME}.elf)
