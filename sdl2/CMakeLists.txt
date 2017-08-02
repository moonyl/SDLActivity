# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

file(GLOB baseSrc src/main/cpp/src/*.c)
file(GLOB addSrc src/main/cpp/src/audio/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/audio/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/audio/dummy/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/core/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/cpuinfo/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/dynapi/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/events/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/file/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/haptic/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/haptic/dummy/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/joystick/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/joystick/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/loadso/dlopen/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/power/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/power/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/filesystem/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/render/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/render/*/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/stdlib/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/thread/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/thread/pthread/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/timer/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/timer/unix/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/video/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/video/android/*.c)
list(APPEND baseSrc ${addSrc})
file(GLOB addSrc src/main/cpp/src/test/*.c)
list(APPEND baseSrc ${addSrc})
list(APPEND baseSrc src/main/cpp/src/atomic/SDL_atomic.c src/main/cpp/src/atomic/SDL_spinlock.c)

add_library( # Sets the name of the library.
             SDL2

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             ${baseSrc}
             )

include_directories( src/main/cpp/include/ )
add_definitions(-DGL_GLEXT_PROTOTYPES)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

find_library( # Sets the name of the path variable.
              dl-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              dl )

find_library( # Sets the name of the path variable.
              GLESv1_CM-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              GLESv1_CM )

find_library( # Sets the name of the path variable.
              GLESv2-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              GLESv2 )

find_library( # Sets the name of the path variable.
              android-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              android )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

#[[
target_link_libraries( # Specifies the target library.
                       main

                       # Links the target library to the log library
                       # included in the NDK.
                       ${SDL2}
                       ${log-lib}
                       ${GLESv1_CM-lib}
                       ${GLESv2-lib})
]]

target_link_libraries( # Specifies the target library.
                       SDL2

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib}
                       ${dl-lib}
                       ${GLESv1_CM-lib}
                       ${GLESv2-lib}
                       ${android-lib})

set(distribution_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../distribution)
set_target_properties(SDL2
                      PROPERTIES LIBRARY_OUTPUT_DIRECTORY
                      "${distribution_DIR}/SDL2/lib/${ANDROID_ABI}")

#[[
add_custom_command(TARGET SDL2 POST_BUILD
                   COMMAND "${CMAKE_COMMAND}" -E
                   copy "${CMAKE_CURRENT_SOURCE_DIR}/src/SDL.h"
                   "${distribution_DIR}/SDL2/include/SDL.h"
                   COMMENT "Copying SDL2 to output directory")
]]