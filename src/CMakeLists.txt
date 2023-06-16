#*****************************************************************************
#   Copyright 2020 Embedded Office GmbH & Co. KG
#
#   Licensed under the Apache License, Version 2.0 (the "License");
#   you may not use this file except in compliance with the License.
#   You may obtain a copy of the License at
#
#       http://www.apache.org/licenses/LICENSE-2.0
#
#   Unless required by applicable law or agreed to in writing, software
#   distributed under the License is distributed on an "AS IS" BASIS,
#   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#   See the License for the specific language governing permissions and
#   limitations under the License.
#******************************************************************************

#---
# define the application executable
#
add_executable(canopen-stm32f3xx)

#---
# application with object dictionary and driver functions
#
target_sources(canopen-stm32f3xx
  PRIVATE
    # application
    app/clock_app.c
    app/clock_hw.c
    app/clock_spec.c
    # system startup
    config/main.c
    config/startup_stm32f303retx.s
    config/system_stm32f3xx.c
    config/stm32f3xx_hal_msp.c
    config/stm32f3xx_it.c
    # driver
    driver/stm32f3xx-hal/drv_can_can1.c
    driver/stm32f3xx-hal/drv_nvm_dummy.c
    driver/stm32f3xx-hal/drv_timer_tim2.c
)

target_include_directories(canopen-stm32f3xx
  PRIVATE
    app
    config
    driver/stm32f3xx-hal
)

#---
# specify the dependencies for this application
#
target_link_libraries(canopen-stm32f3xx canopen-stack stm32f3xx-hal)

#---
# specify the linker file and the resulting image
#
setLinkerScript(canopen-stm32f3xx "${CMAKE_CURRENT_SOURCE_DIR}/config/STM32F303RETX_FLASH")
setOutfile(canopen-stm32f3xx "canopen-stm32f3xx")
