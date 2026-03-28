# CMake Checklist

- 明确版本：写 `cmake_minimum_required(VERSION ...)`。
- 明确语言标准：写 `set(CMAKE_C_STANDARD 17)` 和 `set(CMAKE_C_STANDARD_REQUIRED ON)`。
- 用 target 配置，不要堆全局变量。
- 优先 `target_include_directories`、`target_link_libraries`、`target_compile_definitions`、`target_compile_options`。
- 写清 `PRIVATE`、`PUBLIC`、`INTERFACE`，别让依赖乱传播。
- 少改 `CMAKE_C_FLAGS`，优先改具体 target。
- 用 out-of-source build，源码目录和构建目录分开。
- 少用 `include_directories()`、`link_directories()` 这类全局写法。
- 谨慎用 `file(GLOB ...)` 收集源码，通常直接列文件更稳。
- 不要硬编码库路径和头文件路径，优先 `find_package()` 和导入目标。
- Debug 和 Release 分开配，sanitizer 放 Debug。
- 区分单配置和多配置生成器，不要误用 `CMAKE_BUILD_TYPE`。
- 交叉编译用 toolchain file，不要把平台参数写死在主 `CMakeLists.txt`。
- 测试用 `enable_testing()` 和 `add_test()`。
- 根 `CMakeLists.txt` 保持薄，公共逻辑放子目录或 `cmake/` 目录。
