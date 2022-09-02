cmake_minimum_required(VERSION 3.15)

##############################################
### we use MT for users without C++ runtime
##############################################
if (WIN32)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif ()


##############################################
### at least we support macOS that is still maintained
##############################################
if (APPLE)
    set(CMAKE_OSX_DEPLOYMENT_TARGET "10.14" CACHE STRING "" FORCE)
endif ()


##############################################
### project
##############################################
project(PsdSdk
    VERSION 1.0.0
    DESCRIPTION "Fork of psd_sdk"
    HOMEPAGE_URL "https://github.com/n-taka/psd_sdk"
    LANGUAGES CXX
)


##############################################
### dependencies (None)
##############################################


##############################################
### target
##############################################
# add_library(${PROJECT_NAME} SHARED)
add_executable(${PROJECT_NAME})


##############################################
### source
##############################################
target_sources(${PROJECT_NAME} PRIVATE
    src/Samples/PsdSamples.cpp
    src/Samples/PsdTgaExporter.cpp
    src/Psd/PsdAllocator.cpp
    src/Psd/PsdBitUtil.h
    src/Psd/PsdBlendMode.cpp
    src/Psd/PsdColorMode.cpp
    src/Psd/PsdDecompressRle.cpp
    src/Psd/PsdEndianConversion.h
    src/Psd/PsdExport.cpp
    src/Psd/PsdFile.cpp
    src/Psd/PsdFixedSizeString.cpp
    src/Psd/PsdInterleave.cpp
    src/Psd/Psdispod.h
    src/Psd/PsdLayerCanvasCopy.cpp
    src/Psd/PsdMallocAllocator.cpp
    src/Psd/PsdMemoryUtil.h
    src/Psd/Psdminiz.cpp
    src/Psd/Psdminiz.h
    src/Psd/PsdParseColorModeDataSection.cpp
    src/Psd/PsdParseDocument.cpp
    src/Psd/PsdParseImageDataSection.cpp
    src/Psd/PsdParseImageResourcesSection.cpp
    src/Psd/PsdParseLayerMaskSection.cpp
    src/Psd/PsdPch.cpp
    src/Psd/PsdStringUtil.cpp
    src/Psd/PsdSyncFileReader.cpp
    src/Psd/PsdSyncFileUtil.h
    src/Psd/PsdSyncFileWriter.cpp
    src/Psd/PsdUnionCast.h
    $<$<PLATFORM_ID:Windows>:src/Psd/PsdNativeFile_Windows.cpp>
    $<$<PLATFORM_ID:Linux>:src/Psd/PsdNativeFile_Linux.cpp>
    $<$<PLATFORM_ID:Darwin>:src/Psd/PsdNativeFile_Darwin.mm>
)


##############################################
### include directories (None?)
##############################################
target_include_directories(${PROJECT_NAME} PUBLIC
)


##############################################
### compile options
##############################################
if (WIN32)
    # because we want to use std::filesystem, we require C++17 for windows
    target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_17)
    # our target is windows 10 or newer
    target_compile_definitions(${PROJECT_NAME} PUBLIC _WIN32_WINNT=0x0A00)
    # our codes heavily depend on template...
    add_definitions(/bigobj)
elseif (APPLE)
    # because we support macOS Mojave, we only require C++14 for macOS
    # we use boost::filesystem for macOS
    target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_14)
elseif (UNIX)
    # because we want to use std::filesystem, we require C++17 for Linux
    target_compile_features(${PROJECT_NAME} PUBLIC cxx_std_17)
endif ()
target_compile_definitions(${PROJECT_NAME} PUBLIC _USE_MATH_DEFINES)

