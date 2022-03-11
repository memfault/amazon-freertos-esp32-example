# set(COMPONENT_SRCS
#    cmd_system.c
#    cmd_wifi.c
#    config.c
#    console_example_main.c
#    memfault_platform_device_info.c
# )

# set(COMPONENT_ADD_INCLUDEDIRS .)

# register_component()

idf_component_register(SRCS
cmd_system.c
cmd_wifi.c
config.c
console_example_main.c
memfault_platform_device_info.c

                    INCLUDE_DIRS "")
