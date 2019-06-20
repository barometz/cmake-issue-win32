# Demo of an issue I've been having with CMake + the VS 2019 generator.
#
# Previously my project used the VS2017 generator, which defaults to using Win32
# as the target platform in every place. The VS2019 generator defaults to the
# host platform instead, and my toolchain file is failing to completely set it
# back to Win32.

# Test steps, on an x64 machine with VS2019 installed:
# $ cmake . -DCMAKE_TOOLCHAIN_FILE=x86-windows-msvc.cmake -G "Visual Studio 16 2019"
# $ cmake --build .
# Microsoft (R) Build Engine version 16.1.76+g14b0a930a7 for .NET Framework
# Copyright (C) Microsoft Corporation. All rights reserved.
#
# C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\Microsoft.CppBuild.targets(283,5): error MSB8013: This project doesn't contain the Configuration and Platform combination of Debug|x64. [<...>\CmakeIssue\ZERO_CHECK.vcxproj]

# The toolchain file, which includes a `set(CMAKE_GENERATOR_PLATFORM Win32)`,
# *does* cause Win32 targets to be generated - without it they're all x64. But
# cmake --build then chooses Debug|x64 for no reason I can discern.

# However, and this is where I got extra confused, when I pass that option via
# the command line (either `-DCMAKE_GENERATOR_PLATFORM=Win32` or `-A Win32`)
# everything works exactly fine.

# Lowest version our dev systems have, 3.14 doesn't help
cmake_minimum_required(VERSION 3.13)
project(TestCmakeIssue CXX)
add_executable(test_cmake_issue main.cpp)
