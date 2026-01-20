# Glad2-CMake

This is a convenience wrapper in CMake of the original library created by [Dav1dde](https://github.com/Dav1dde/glad).
I created it so that Glad can be used with CMake's FetchContent or vcpkg instead of having it as loose files and as extra
CMake target in the project. 

OpenGL Loader for OpenGL 4.6 generated with the [Glad2 Generator](https://gen.glad.sh/#generator=c&api=gl%3D4.6&profile=gl%3Dcore%2Cgles1%3Dcommon).

## Requirements

- CMake 3.30+
- C++23 compatible compiler

## Usage

To use it with vcpkg, you need to add it as custom port to your vcpkg project.

1. Create a subdirectory for ports

    ```bash
    mkdir vcpkg-ports
    cd vcpkg-ports
    ```

2. Create a subdirectory for Glad2-CMake

    ```bash
    mkdir glad2cmake
    cd glad2cmake
    ```

3. Set up vcpkg.json

    ```json
    {
        "name": "glad2cmake",
        "version-string": "1.1.5",
        "description": "Glad2 OpenGL loader packaged for CMake",
        "homepage": "https://github.com/FelixHommel/Glad2-CMake",
        "license": "MIT",
        "dependencies": [
            {
                "name": "vcpkg-cmake",
                "host": true
            },
            {
                "name": "vcpkg-cmake-config",
                "host": true
            }
        ]
    }
    ```

4. Set up portfolio.cmake

    ```cmake
    vcpkg_from_github(
        OUT_SOURCE_PATH SOURCE_PATH
        REPO FelixHommel/Glad2-CMake
        REF v1.1.5
        SHA512 <hash of the release archive>
    )

    vcpkg_cmake_configure(
        SOURCE_PATH "${SOURCE_PATH}"
    )

    vcpkg_cmake_install()

    vcpkg_cmake_config_fixup(
        PACKAGE_NAME glad2cmake
        CONFIG_PATH lib/cmake/glad2cmake
    )

    vcpkg_install_copyright(FILE_LIST "${SOURCE_PATH}/LICENSE")
    ```

5. Add the custom ports to the root vcpkg.json

    ```json
    "overlay-ports": [
        "vcpkg-ports"
    ]
    ```
