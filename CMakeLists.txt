cmake_minimum_required(VERSION 3.5)

# =========================================================
# Fix for Windows: Auto-detect and normalize Python path
# 自动检测 Python 路径并强制转换为 CMake 兼容格式（正斜杠）
# =========================================================
if(NOT PYTHON)
    find_program(PYTHON_AUTO_DETECT NAMES python python3)
    if(PYTHON_AUTO_DETECT)
        set(PYTHON "${PYTHON_AUTO_DETECT}")
    endif()
endif()

if(PYTHON)
    # 将 Windows 的反斜杠 \ 转换为 CMake 需要的正斜杠 /
    file(TO_CMAKE_PATH "${PYTHON}" PYTHON_FIXED)
    # 强制写入缓存，确保子项目（如 Bootloader）也能继承此路径
    set(PYTHON "${PYTHON_FIXED}" CACHE STRING "Python Path" FORCE)
    message(STATUS "Build System: Forced Python path to ${PYTHON}")
endif()
# =========================================================

# Colored  Compiler Output with ninja
add_compile_options (-fdiagnostics-color=always)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})

# 加载 ESP-IDF SDK (此时 PYTHON 变量已经被修复)
include($ENV{IDF_PATH}/tools/cmake/project.cmake)

project(wireless_esp_dap)
