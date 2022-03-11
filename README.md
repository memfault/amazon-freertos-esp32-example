# Amazon FreeRTOS ESP32 Example

This is an example project for the ESP32 Wrover kit, based around the Amazon
FreeRTOS SDK.

## Building the Example

This example is based loosely around the instructions here (which are incomplete
and do not work with the latest Amazon FreeRTOS tags):

https://docs.aws.amazon.com/freertos/latest/userguide/getting_started_espressif.html#getting_started_espressif_cmake_project

To run the example (this assumes a unix-like bash environment- tested on Ubuntu
linux, but should be similar for Mac osx or Windows git bash shell), follow
these instructions:

### Prerequisites

See the setup instructions here:

https://docs.aws.amazon.com/freertos/latest/userguide/getting_started_espressif.html#setup-espressif-idf42

We don't care about all the AWS stuff, just the ESP-IDF setup

1. install prerequisites for your platform, following step 1 from here:

   https://docs.espressif.com/projects/esp-idf/en/release-v4.2/esp32/get-started/index.html#step-1-install-prerequisites

### Build Instructions

1. first clone this repo with submodules:

   ```bash
   # --recursive to get submodules too. warning, this takes several minutes!
   ❯ git clone --recursive https://github.com/memfault/amazon-freertos-esp32-example.git
   ```

2. cd into the freshly cloned repo and install the esp toolchain:

   ```bash
   ❯ freertos/vendors/espressif/esp-idf/install.sh
   ```

3. activate the esp build environment:

   ```bash
   ❯ export IDF_PATH=$(pwd)/freertos/vendors/espressif/esp-idf/ && source freertos/vendors/espressif/esp-idf/export.sh
   ```

4. build the project:

   ```bash
   ❯ cmake -B build -DIDF_SDKCONFIG_DEFAULTS=$(pwd)/sdkconfig.defaults \
     -DCMAKE_TOOLCHAIN_FILE=freertos/tools/cmake/toolchains/xtensa-esp32.cmake \
     -GNinja --log-level=status
   ```

5. flash the result to the board:

   ```bash
   # flash the application and start the serial port monitor
   ❯ idf.py flash monitor
   ```
