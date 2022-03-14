idf_component_register(
    SRCS
    cmd_system.c
    cmd_wifi.c
    config.c
    console_example_main.c
    memfault_platform_device_info.c

    INCLUDE_DIRS
    "."

    REQUIRES
    console
    esp_https_ota
    fatfs
    memfault
    nvs_flash
    tcpip_adapter
    # freertos_tcpip
)
