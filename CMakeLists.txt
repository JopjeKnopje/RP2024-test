cmake_minimum_required(VERSION 3.13)
# initialize the SDK based on PICO_SDK_PATH
# note: this must happen before project()
include(pico_sdk_import.cmake)

project(my_project)

# initialize the Raspberry Pi Pico SDK
pico_sdk_init()

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
# rest of your project
if (TARGET tinyusb_device)
    add_executable(firmware
            src/main.c
            )

    # pull in common dependencies
    target_link_libraries(firmware pico_stdlib)

    # enable usb output, disable uart output
    pico_enable_stdio_usb(firmware 1)
    pico_enable_stdio_uart(firmware 0)

    # create map/bin/hex/uf2 file etc.
    pico_add_extra_outputs(firmware)

    # add url via pico_set_program_url
    # example_auto_set_url(firmware)
elseif(PICO_ON_DEVICE)
    message("Skipping hello_usb because TinyUSB submodule is not initialized in the SDK")
endif()
