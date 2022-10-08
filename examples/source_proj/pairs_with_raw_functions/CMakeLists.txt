# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
include(../../../CPlatform.txt)

set(CMAKE_CXX_STANDARD 17)  # SOL库必须要支持c++17

set(PROJECT_NAME pairs_with_raw_functions)

################################################################################
# Source groups
################################################################################

file(
    GLOB ALL_FILES
    "../../source/pairs_with_raw_functions.cpp"
)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${ROOT_PATH}/bin/)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${ROOT_PATH}/bin/)

################################################################################
# Target
################################################################################
add_executable(${PROJECT_NAME} ${ALL_FILES})
set_target_properties(${PROJECT_NAME} PROPERTIES FOLDER "examples/source")

################################################################################
# Include directories
################################################################################
if("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Any CPU")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "Win32")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
    )
elseif("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64")
    target_include_directories(${PROJECT_NAME} PRIVATE
	    "${ROOT_PATH}/lua-5.4.4/src"
        "${ROOT_PATH}/include"
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

################################################################################
# Compile and link options
################################################################################
include(../../../VSCompileLink.txt)
