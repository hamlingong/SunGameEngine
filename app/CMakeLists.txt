# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( v8_base
             STATIC
             IMPORTED )
set_target_properties( # Specifies the target library.
                         v8_base

                       # Specifies the parameter you want to define.
                       # Provides the path to the library you want to import.
                       PROPERTIES IMPORTED_LOCATION  ${CMAKE_SOURCE_DIR}/src/main/cpp/${ANDROID_ABI}/libv8_base.a )


add_library( v8_snapshot
             STATIC
             IMPORTED )
set_target_properties( # Specifies the target library.
                         v8_snapshot

                       # Specifies the parameter you want to define.
                       # Provides the path to the library you want to import.
                       PROPERTIES IMPORTED_LOCATION  ${CMAKE_SOURCE_DIR}/src/main/cpp/${ANDROID_ABI}/libv8_snapshot.a )

add_library( # Sets the name of the library.
             sun

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             # graphics
             src/main/cpp/graphics/GLUtils.h
             src/main/cpp/graphics/GLUtils.cpp
             src/main/cpp/graphics/Matrix.h
             src/main/cpp/graphics/Matrix.cpp

             #util
             src/main/cpp/util/v8util.h
             src/main/cpp/util/v8util.cpp


             # engine source
             src/main/cpp/sun.cpp
             src/main/cpp/sun_engine.cpp
             src/main/cpp/sun_engine.h
             src/main/cpp/log.h
         )

include_directories(src/main/cpp/
                    src/main/cpp/include
                    )

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

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       sun

                       # Links the target library to the log library
                       # included in the NDK.
                       EGL
                       GLESv2
                       android
                       ${log-lib}
                       v8_base
                       v8_snapshot)