cmake_minimum_required(VERSION 3.20)

# You can set the name of your project here
project(ImageEditor)
add_executable(${PROJECT_NAME})

# Choose your C++ version
target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)

# Prevents compiler-specific extensions to C++ because they might allow code to compile on your machine but not on other people's machine
set_target_properties(${PROJECT_NAME} PROPERTIES
    CXX_EXTENSIONS OFF)

# Add all the source files
file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS src/* lib/*)
target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE src lib)

# Download the sil library
include(FetchContent)
FetchContent_Declare(
    sil
    GIT_REPOSITORY https://github.com/JulesFouchy/Simple-Image-Lib
    GIT_TAG 89bd175cd55fb6daa9e99b218e869c3d516043f4
)
FetchContent_MakeAvailable(sil)

# Link sil into the project
target_link_libraries(${PROJECT_NAME} PRIVATE sil)