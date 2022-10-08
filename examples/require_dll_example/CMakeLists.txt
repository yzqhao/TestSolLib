# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
include(../../CPlatform.txt)

set(CMAKE_CXX_STANDARD 17)  # SOL库必须要支持c++17

set(PROJECT_NAME_DLL my_object)
set(PROJECT_NAME require_dll_example)

################################################################################
# Source groups
################################################################################

file(
    GLOB ALL_FILES_DLL
    "./source/my_object.cpp"
    "./include/my_object/*.hpp"
)

file(
    GLOB ALL_FILES
    "./source/require_from_dll.cpp"
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${ROOT_PATH}/bin/)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${ROOT_PATH}/bin/)

################################################################################
# Target
################################################################################

set(CMAKE_DEBUG_POSTFIX "")

add_library(${PROJECT_NAME_DLL} ${LIBRARY_TYPE_BY_PLATFORM} ${ALL_FILES_DLL})
set_target_properties(${PROJECT_NAME_DLL} PROPERTIES FOLDER "examples/require_dll")

add_executable(${PROJECT_NAME} ${ALL_FILES})
set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "examples/require_dll")

################################################################################
# Include directories
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Any CPU")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
    target_include_directories(${PROJECT_NAME_DLL} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
    target_include_directories(${PROJECT_NAME_DLL} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
    target_include_directories(${PROJECT_NAME_DLL} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
	    "${ROOT_PATH}/examples/require_dll_example/include"
    )
endif()

################################################################################
# Dependencies
################################################################################
add_dependencies(${PROJECT_NAME}
	lua
	)
target_link_libraries(${PROJECT_NAME} PUBLIC
    lua
)
add_dependencies(${PROJECT_NAME_DLL}
	lua
	)
target_link_libraries(${PROJECT_NAME_DLL} PUBLIC
    lua
)	



################################################################################
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME_DLL} PRIVATE
    "$<$<CONFIG:Debug>:"
        "DEBUG;"
    ">"
    "$<$<CONFIG:Release>:"
    ">"
    "WIN32;"
    "_CRT_SECURE_NO_WARNINGS;"
    "UNICODE;"
    "_UNICODE"
	"MY_OBJECT_DLL"
	"MY_OBJECT_BUILD"
)	

################################################################################
# Compile and link options
################################################################################
include(../../VSCompileLink.txt)
