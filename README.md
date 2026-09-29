# pygems

Python Generic Embedded System for C++

## Building

```bash
git clone https://github.com/vg-lab/pygems.git
cd pygems

cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build --parallel
```

In-source builds are not allowed, so always configure into a separate build directory. If `CMAKE_BUILD_TYPE` is not specified, it defaults to `Debug`, so make sure to build the `Release` version to get the best performance possible.

## CMake options

| Option | Default | Description |
| ------ | ------- | ----------- |
| `BUILD_SHARED_LIBS` | `ON` | Build pygems as a shared library (set to `OFF` for static) |
| `BUILD_DOCS` | `ON` | Generate Doxygen documentation when Doxygen is found |
| `PYGEMS_WITH_EXAMPLES` | `ON` | Build the programs in `examples/` (only when pygems is the top-level project) |
| `PYGEMS_OPTIONALS_AS_REQUIRED` | `OFF` | Treat optional dependencies as required |
| `PYGEMS_USE_PYTHON3` | `ON` | Build against Python 3 |

Example:

```bash
cmake -S . -B build -DBUILD_SHARED_LIBS=ON -DPYGEMS_WITH_EXAMPLES=OFF -DBUILD_DOCS=OFF
```

## Dependencies

pygems resolves its dependencies through CMake, using `find_package()`. Dependencies are not downloaded automatically, so they must be installed on your system or made discoverable by adding their install location to `CMAKE_PREFIX_PATH`.

## Using pygems in your project

`pygems` and `pygems::pygems` refer to the same CMake library target; the namespaced form is recommended for downstream projects.

### With `find_package`

Install pygems, then consume it:

```bash
cmake --install build --prefix /your/prefix
```

```cmake
find_package(pygems REQUIRED)
target_link_libraries(my_app PRIVATE pygems::pygems)
```

Add `-DCMAKE_PREFIX_PATH=/your/prefix` when configuring your project if the prefix is not a default search location.

### With `FetchContent`

```cmake
include(FetchContent)
FetchContent_Declare(
  pygems
  GIT_REPOSITORY https://github.com/vg-lab/pygems.git
  GIT_TAG        master  # pin to a tag or commit
)
FetchContent_MakeAvailable(pygems)

target_link_libraries(my_app PRIVATE pygems::pygems)
```

## Documentation

The API documentation is generated from the source with Doxygen. To build it locally (requires Doxygen and BUILD_DOCS=ON):

```bash
cmake --build build --target pygems_doxygen
```

The HTML output is written to build/docs_doxygen/html.

## License

pygems is released under the [GNU General Public License v3.0](LICENSE.txt).

Developed at the [Visualization and Graphics Lab (VG-Lab)](https://github.com/vg-lab).
