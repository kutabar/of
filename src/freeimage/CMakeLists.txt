cmake_minimum_required(VERSION 2.6)

project(freeimage)

find_package(ZLIB REQUIRED)

if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE "Release")
endif()

if(UNIX AND NOT APPLE)
   set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY
     ${CMAKE_CURRENT_SOURCE_DIR}/../../lib/${CMAKE_BUILD_TYPE}/linux)
elseif(WIN32)
   set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY
     ${CMAKE_CURRENT_SOURCE_DIR}/../../lib/${CMAKE_BUILD_TYPE}/windows)
endif()

# Set the output paths
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "bin/")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "bin/")

# Process subdirectories
add_subdirectory(LibJPEG)
add_subdirectory(LibOpenJPEG)
add_subdirectory(LibPNG)
add_subdirectory(LibTIFF4)
add_subdirectory(OpenEXR/Half)

# List the header files
set(HEADERS CacheFile.h
            FreeImage.h
            FreeImageIO.h
            Plugin.h
            Quantizers.h
            ToneMapping.h
            Utilities.h
            FreeImage/PSDParser.h
            FreeImageToolkit/Filters.h
            FreeImageToolkit/Resize.h
            Metadata/FIRational.h
            Metadata/FreeImageTag.h
)

# List the source files
set(SRCS FreeImage/BitmapAccess.cpp
         FreeImage/CacheFile.cpp
         FreeImage/ColorLookup.cpp
         FreeImage/Conversion.cpp
         FreeImage/Conversion16_555.cpp
         FreeImage/Conversion16_565.cpp
         FreeImage/Conversion24.cpp
         FreeImage/Conversion32.cpp
         FreeImage/Conversion4.cpp
         FreeImage/Conversion8.cpp
         FreeImage/ConversionFloat.cpp
         FreeImage/ConversionRGB16.cpp
         FreeImage/ConversionRGBF.cpp
         FreeImage/ConversionType.cpp
         FreeImage/ConversionUINT16.cpp
         FreeImage/FreeImage.cpp
         FreeImage/FreeImageIO.cpp
         FreeImage/GetType.cpp
         FreeImage/Halftoning.cpp
         FreeImage/J2KHelper.cpp
         FreeImage/MemoryIO.cpp
         FreeImage/MNGHelper.cpp
         FreeImage/MultiPage.cpp
         FreeImage/NNQuantizer.cpp
         FreeImage/PSDParser.cpp
         FreeImage/PixelAccess.cpp
         FreeImage/Plugin.cpp
         FreeImage/PluginBMP.cpp
         FreeImage/PluginCUT.cpp
         FreeImage/PluginDDS.cpp
         FreeImage/PluginG3.cpp
         FreeImage/PluginGIF.cpp
         FreeImage/PluginHDR.cpp
         FreeImage/PluginICO.cpp
         FreeImage/PluginIFF.cpp
         FreeImage/PluginJ2K.cpp
         FreeImage/PluginJNG.cpp
         FreeImage/PluginJP2.cpp
         FreeImage/PluginJPEG.cpp
         FreeImage/PluginKOALA.cpp
         FreeImage/PluginMNG.cpp
         FreeImage/PluginPCD.cpp
         FreeImage/PluginPCX.cpp
         FreeImage/PluginPFM.cpp
         FreeImage/PluginPICT.cpp
         FreeImage/PluginPNG.cpp
         FreeImage/PluginPNM.cpp
         FreeImage/PluginPSD.cpp
         FreeImage/PluginRAS.cpp
         FreeImage/PluginSGI.cpp
         FreeImage/PluginTARGA.cpp
         FreeImage/PluginTIFF.cpp
         FreeImage/PluginWBMP.cpp
         FreeImage/PluginXBM.cpp
         FreeImage/PluginXPM.cpp
         FreeImage/TIFFLogLuv.cpp
         FreeImage/ToneMapping.cpp
         FreeImage/WuQuantizer.cpp
         FreeImage/ZLibInterface.cpp
         FreeImage/tmoColorConvert.cpp
         FreeImage/tmoDrago03.cpp
         FreeImage/tmoFattal02.cpp
         FreeImage/tmoReinhard05.cpp
         FreeImageToolkit/BSplineRotate.cpp
         FreeImageToolkit/Background.cpp
         FreeImageToolkit/Channels.cpp
         FreeImageToolkit/ClassicRotate.cpp
         FreeImageToolkit/Colors.cpp
         FreeImageToolkit/CopyPaste.cpp
         FreeImageToolkit/Display.cpp
         FreeImageToolkit/Flip.cpp
         FreeImageToolkit/JPEGTransform.cpp
         FreeImageToolkit/MultigridPoissonSolver.cpp
         FreeImageToolkit/Rescale.cpp
         FreeImageToolkit/Resize.cpp
         Metadata/Exif.cpp
         Metadata/FIRational.cpp
         Metadata/FreeImageTag.cpp
         Metadata/IPTC.cpp
         Metadata/TagConversion.cpp
         Metadata/TagLib.cpp
         Metadata/XTIFF.cpp
)

# List the include paths
include_directories(${CMAKE_SOURCE_DIR}
                    ${CMAKE_SOURCE_DIR}/DeprecationManager
)

include_directories(${ZLIB_INCLUDE_DIRS})

# Declaration of the library
add_library(freeimage STATIC ${HEADERS} ${SRCS})

set_target_properties(freeimage PROPERTIES COMPILE_DEFINITIONS "OPJ_STATIC;FREEIMAGE_LIB") 
set_target_properties(freeimage PROPERTIES COMPILE_FLAGS "-w -fPIC -fpermissive")

target_link_libraries(freeimage jpeg mng openjpeg png tiff ${ZLIB_LIBRARIES})
add_dependencies(freeimage jpeg mng openjpeg png tiff ${ZLIB_LIBRARIES})
