# wbenny-ziplib-cmake

CMake version of wbenny's ziplib, for vcpkg

Original Visual Studio project: <https://bitbucket.org/wbenny/ziplib>

## Build as static library

Clone repo, then from the repo dir:

```
mkdir build
cd build
cmake ..
make
```

## Use library as part of a CMake project

* Clone the repo to a subdirectory, for example `ziplib`, optionally as a git submodule
* From the main project's CMakeLists.txt:
  * `add_subdirectory(ziplib)`
  * `target_link_libraries(${PROJECT_NAME} PUBLIC ziplib)`
* From the main project's source files, use #include directives from the `Source` dir, for example `#include <ziplib/Source/ZipLib/ZipFile.h>`