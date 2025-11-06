
# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.2.0)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.2.0-a4)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
cmake_minimum_required(VERSION 3.13)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(PICO_BOARD pico_w CACHE STRING "Board type")
include(pico_sdk_import.cmake)
project(DualCore-RP2040 C CXX ASM)
pico_sdk_init()

include_directories( ${CMAKE_SOURCE_DIR}/lib ) 

add_executable(DualCore-RP2040 DualCore-RP2040.c lib/bmp280.c lib/aht20.c lib/ssd1306.c lib/alerts.c)

pico_set_program_name(DualCore-RP2040 "DualCore-RP2040")
pico_set_program_version(DualCore-RP2040 "0.1")

pico_enable_stdio_uart(DualCore-RP2040 0)
pico_enable_stdio_usb(DualCore-RP2040 0)

target_link_libraries(DualCore-RP2040
        pico_stdlib
        pico_multicore
        hardware_gpio
        hardware_adc
        hardware_pwm
        hardware_pio
        hardware_i2c
        hardware_clocks)

# Add the standard include files to the build
target_include_directories(DualCore-RP2040 PRIVATE
        ${CMAKE_CURRENT_LIST_DIR}
)

pico_add_extra_outputs(DualCore-RP2040)