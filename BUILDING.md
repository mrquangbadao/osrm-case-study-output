# OSRM BACKEND BUILDING ON WINDOWS 

To build the OSRM (Open Source Routing Machine) backend using vcpkg and CMake on Windows, follow these steps:

## Prerequisites

1. Install Visual Studio: Ensure you have Visual Studio installed with the C++ development tools.
Currently used Visual Studio 17 2022 with C++ development features.

2. Install CMake: If you haven't already, go to the CMake download page(https://cmake.org/download/) download and run the Windows installer.

3. Install vcpkg: If you haven't already, clone and bootstrap vcpkg.

```bash
#!/bin/bash
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat
```

## Builing From Source Step-by-Step Guide

1. ***Install Dependencies with vcpkg***: OSRM has several dependencies that need to be installed. Use vcpkg to install them.

```bash
#!/bin/bash
cd vcpkg
vcpkg install boost tbb lua BZIP2 EXPAT
vcpkg integrate install
```

2. ***Clone the OSRM Backend Repository***: Clone the OSRM backend repository from GitHub.

```bash
#!/bin/bash
git clone https://github.com/Project-OSRM/osrm-backend.git
cd osrm-backend
```

***Noted***: There is a bug in guidance.lua file in the original source code. So, we have to fix this before building:
- Open ..\osrm-backend\profiles\lib\guidance.lua
- In line 112, change from: 

```c++
result.road_classification.num_lanes = lc
```

To: 

```c++
result.road_classification.num_lanes = math.floor(lc)
```

- Save and re-build.

3. ***Configure the Project with CMake***: Use CMake to configure the project. Make sure to specify the vcpkg toolchain file.

```bash
mkdir build
cd build
cmake -DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake ..
```

Replace [path to vcpkg] with the actual path to your vcpkg directory.

4. ***Build the Project***: Use CMake to build the project.

```bash
# Build the project
cmake --build . --config Release
```

***Example Commands***:
Here is a complete example of the commands you might run in sequence:

```bash
# Clone vcpkg and bootstrap it
git clone https://github.com/microsoft/vcpkg.git
cd vcpkg
.\bootstrap-vcpkg.bat

# Install dependencies
vcpkg install boost tbb lua BZIP2 EXPAT
vcpkg integrate install

# Clone OSRM backend
git clone https://github.com/Project-OSRM/osrm-backend.git
cd osrm-backend

# Fixing guidance.lua error code before do the next step

# Create build directory and configure with CMake
mkdir build
cd build
cmake -DCMAKE_TOOLCHAIN_FILE=../../vcpkg/scripts/buildsystems/vcpkg.cmake ..

# Build the project
cmake --build . --config Release

```

After successfully building the OSRM backend, the output target will be under the following folder: 

```
..\osrm-backend\build\Release
```

Additionally, we can copy the 'profiles' and 'data' folder under the source code to this output build folder for a demo run. For demo run, please follow the instructions from [here](README.md)
