# Packaging

## Building CMSIS_5 Math libraries
CMSIS_5 doesn't come with prebuilt .lib files, so we will need to build them ourselves. The build process is a little convoluted right now, but fairly straightforward.

Start by creating a build folder inside the CMSIS_5/CMSIS/DSP folder. In the same DSP folder, create a CMakeLists.txt file with the following content:

```
cmake_minimum_required (VERSION 3.14)

# Define the project
project (buildcmsisdsp VERSION 0.1)

# Define the path to CMSIS-DSP
set(DSP ${CMAKE_CURRENT_SOURCE_DIR})

# Add DSP folder to module path
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})
list(APPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/Source)

########### 
#
# CMSIS DSP
#

# Load CMSIS-DSP definitions. Libraries will be built in bin_dsp
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/Source bin_dsp)
```

From powershell on Windows, cd into the build folder, and run 

```bash
cmake -DROOT="../../../.." -DCMAKE_TOOLCHAIN_FILE="../gcc.cmake" -DARM_CPU="cortex-m0plus" -DCMAKE_C_COMPILER="C:/Program Files (x86)/GNU Arm Embedded Toolchain/10 2020-q4-major/bin/arm-none-eabi-gcc.exe" -DCMAKE_CXX_COMPILER="C:/Program Files (x86)/GNU Arm Embedded Toolchain/10 2020-q4-major/bin/arm-none-eabi-gcc.exe" -DCMAKE_MAKE_PROGRAM="C:/Program Files (x86)/GnuWin32/bin/make.exe" -G "Unix Makefiles" ..
```

note that you'll need GnuWin32 make installed, as well as the GNU Arm embedded toolchain. On linux, the command is very similar, but I haven't tested it yet.

After that, run 

```bash
make VERBOSE=1
```

That should build .a files for each of the math libraries, in the build folder. 

## Packaging instructions

**Make sure to build the math libraries FIRST**

To package this repository into the required files for the Arduino IDE, run ```make all```. In linux this *should* just work (untested), for Windows you'll need to install Cygwin with the Perl package selected and run it from the Cygwin terminal. 