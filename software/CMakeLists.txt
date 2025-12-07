cmake_minimum_required(VERSION 3.12)

set(PICO_SDK_PATH ../lib/pico-sdk)
include(lib/pico-sdk/external/pico_sdk_import.cmake)

project(VersaTerm C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
pico_sdk_init()

add_compile_options(-Wall)

# To change the default serial config for all apps, pass e.g.
#   cmake -DDVI_DEFAULT_SERIAL_CONFIG=pimoroni_demo_hdmi_cfg ..
# then rebuild. This is applied via target_compile_definitions on each app
# individually.
# set(DVI_DEFAULT_SERIAL_CONFIG "pico_sock_cfg" CACHE STRING
# set(DVI_DEFAULT_SERIAL_CONFIG "waveshare_rp2040_pizero" CACHE STRING
#	"Select a default pin configuration from common_dvi_pin_configs.h")

# Detect pico boards with rp2350B
if(PICO_BOARD STREQUAL "waveshare_rp2350_pizero")
	set(DVI_DEFAULT_SERIAL_CONFIG "waveshare_rp2350_pizero")
	set(DVI_SERIAL_USES_LARGE_GPIO 1)
	add_compile_definitions(PICO_PIO_USE_GPIO_BASE=1)
elseif(PICO_BOARD STREQUAL "waveshare_rp2040_pizero")
	set(DVI_DEFAULT_SERIAL_CONFIG "waveshare_rp2040_pizero")
	set(DVI_SERIAL_USES_LARGE_GPIO 0)
	add_compile_definitions(PICO_PIO_USE_GPIO_BASE=0)
else()
	set(DVI_DEFAULT_SERIAL_CONFIG "unknown")
	set(DVI_SERIAL_USES_LARGE_GPIO 0)
	add_compile_definitions(PICO_PIO_USE_GPIO_BASE=0)
endif()

include_directories(
	lib/PicoDVI/software/include
	)

add_subdirectory(lib/PicoDVI/software/libdvi)
add_subdirectory(src)
