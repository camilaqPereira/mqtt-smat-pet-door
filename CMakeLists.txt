cmake_minimum_required(VERSION 3.13)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

project(smartpet_door_mqtt_client C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(smartpet_door_mqtt_client smartpet_door_mqtt_client.c 
    lib/hc_sr04.c
    lib/servomotor.c
)

pico_set_program_name(smartpet_door_mqtt_client "smartpet_door_mqtt_client")
pico_set_program_version(smartpet_door_mqtt_client "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(smartpet_door_mqtt_client 0)
pico_enable_stdio_usb(smartpet_door_mqtt_client 1)

# Add the standard library to the build
target_link_libraries(smartpet_door_mqtt_client
    pico_stdlib
    hardware_pwm
    pico_cyw43_arch_lwip_threadsafe_background
    pico_lwip_mqtt
    pico_mbedtls
    pico_lwip_mbedtls
    )

# Add the standard include files to the build
target_include_directories(smartpet_door_mqtt_client PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)

# Add any user requested libraries
target_link_libraries(smartpet_door_mqtt_client 
    )

pico_add_extra_outputs(smartpet_door_mqtt_client)

