cmake_minimum_required(VERSION 3.12)
project(GroundExtractor)

# Add your source files here
set(SOURCE_FILES
    GroundExtractor/GroundExtraction.cpp
    GroundExtractor/LasInput.cpp
    GroundExtractor/TerrainFilter.cpp
    GroundExtractor/BlockProcessor.cpp 
    # Add more source files as needed
)
set(HEADER_FILES 
    GroundExtractor/LH.h
    GroundExtractor/LasInput.h
    GroundExtractor/TerrainFilter.h
    GroundExtractor/BlockProcessor.h
)

# Set the executable name
add_executable(GroundExtractor ${SOURCE_FILES} ${HEADER_FILES})

# Specify the C++ standard
set_property(TARGET GroundExtractor PROPERTY CXX_STANDARD 11)

# Find OpenCV package
find_package(OpenCV 4.2 REQUIRED)
message("OpenCV VERSION:")
find_package(PCL CONFIG REQUIRED)

# Specify include directories if needed
include_directories(${HEADER_FILES})
include_directories(${PCL_INCLUDE_DIRS})
include_directories(${OpenCV_INCLUDE_DIRS})
include_directories(../Fade2D/include_fade25d)


link_directories(
    ${PCL_LIBRARY_DIRS}
    ${OpenCV_LIBRARY_DIRS}
    ../Fade2D/include_fade25d
)
# Specify the C++ standard
set_property(TARGET GroundExtractor PROPERTY CXX_STANDARD 11)

# Link against OpenCV libraries
target_link_libraries(GroundExtractor 
    ${PCL_LIBRARIES}
    ${OpenCV_LIBS}
)

find_library(FADE25D_LIBRARY fade25d PATHS ../Fade2D/lib_ubuntu16.04_x86_64)
if (NOT FADE25D_LIBRARY)
    message(FATAL_ERROR "Fade2D library not found. Set the correct path using -DFADE25D_LIBRARY.")
endif()


target_link_libraries(GroundExtractor ${FADE25D_LIBRARY})
