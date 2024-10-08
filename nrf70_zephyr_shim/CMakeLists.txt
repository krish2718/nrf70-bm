#
# Copyright (c) 2024 Nordic Semiconductor ASA
#
# SPDX-License-Identifier: BSD-3-Clause
#

# Check if ZEPHYR_BASE is set
if(NOT DEFINED ENV{ZEPHYR_BASE})
    message(FATAL_ERROR "ZEPHYR_BASE environment variable is not set. Please set it to the Zephyr base directory.")
endif()

if (CONFIG_NRF70_ZEPHYR_SHIM)
  set(NRF_DRV_DIR ${ZEPHYR_CURRENT_MODULE_DIR}/../nrf/drivers/wifi/nrf700x)
  set(NRF_WIFI_DIR ${ZEPHYR_CURRENT_MODULE_DIR}/../nrfxlib/nrf_wifi)

  zephyr_library_named(nrf70-zep-shim)

  zephyr_library_include_directories(
    include
    ${NRF_WIFI_DIR}/os_if/inc
    ${NRF_WIFI_DIR}/hw_if/hal/inc/fw/
  )


  zephyr_library_sources(source/platform/rpu_hw_if.c)

  zephyr_library_sources_ifdef(CONFIG_NRF700X_ON_QSPI
    ${NRF_DRV_DIR}/src/qspi/src/qspi_if.c
  )
  zephyr_library_sources_ifdef(CONFIG_NRF700X_ON_SPI
    source/bus/spi_if.c
  )
  zephyr_library_sources(source/bus/device.c)

  zephyr_library_sources(source/os/shim.c)
  zephyr_library_sources(source/os/work.c)
  zephyr_library_sources(source/os/timer.c)
endif()
