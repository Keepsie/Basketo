cmake_minimum_required(VERSION 3.10)
project(BasketoGameEngine)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


add_subdirectory(vendor/sol2)

set(SDL2_INCLUDE_DIRS "C:/Libraries/SDL2")
set(SDL2_LIBRARIES "C:/Libraries/SDL2/SDL2.lib" "C:/Libraries/SDL2/SDL2main.lib")

set(SDL2_IMAGE_INCLUDE_DIRS "C:/Libraries/SDL2_image")
set(SDL2_IMAGE_LIBRARIES "C:/Libraries/SDL2_image/SDL2_image.lib")

set(SDL2_TTF_INCLUDE_DIRS "C:/Libraries/SDL2_ttf")
set(SDL2_TTF_LIBRARIES "C:/Libraries/SDL2_ttf/SDL2_ttf.lib")

set(SDL2_MIXER_INCLUDE_DIRS "C:/Libraries/SDL2_mixer")
set(SDL2_MIXER_LIBRARIES "C:/Libraries/SDL2_mixer/SDL2_mixer.lib")

set(LUA_INCLUDE_DIRS "C:/Libraries/Lua/include" CACHE PATH "Path to Lua 5.4 include directory")
set(LUA_LIBRARIES "C:/Libraries/Lua/lua54.lib" CACHE PATH "Path to Lua 5.4 library")

set(SOL_INCLUDE_DIRS "C:/sol" CACHE PATH "Path to sol2 include directory")

find_package(OpenGL REQUIRED)
include_directories(${OpenGL_INCLUDE_DIRS})
include_directories(${SDL2_INCLUDE_DIRS})
include_directories(${SDL2_IMAGE_INCLUDE_DIRS})
include_directories(${SDL2_TTF_INCLUDE_DIRS})
include_directories(${SDL2_MIXER_INCLUDE_DIRS})
include_directories(${LUA_INCLUDE_DIRS})
include_directories(${SOL_INCLUDE_DIRS})
include_directories(vendor/imgui)
include_directories(vendor/libtinyfiledialogs)

set(SOURCE_FILES
    src/main.cpp
    src/Game.cpp
    src/InputManager.cpp
    src/SceneManager.cpp
    src/AssetManager.cpp
    src/scenes/GameScene.cpp
    src/scenes/MenuScene.cpp
    src/scenes/DevModeScene.cpp
    src/scenes/DevModeInputHandler.cpp
    src/scenes/DevModeSceneSerializer.cpp
    
    src/scenes/InspectorPanel.cpp
    utils/utility.cpp
    src/Physics.cpp

    vendor/imgui/imgui.cpp
    vendor/imgui/imgui_demo.cpp
    vendor/imgui/imgui_draw.cpp
    vendor/imgui/imgui_tables.cpp
    vendor/imgui/imgui_widgets.cpp
    vendor/imgui/backends/imgui_impl_sdl2.cpp
    vendor/imgui/backends/imgui_impl_sdlrenderer2.cpp
    vendor/libtinyfiledialogs/tinyfiledialogs.c
    src/ecs/systems/ScriptSystem.cpp
    src/ecs/systems/AnimationSystem.cpp
    src/utils/EditorUtils.cpp
    src/utils/FileUtils.cpp
    src/utils/Console.cpp
)

add_executable(PhysicsTest
    tests/test_physics.cpp
    src/Physics.cpp
    utils/utility.cpp
)

add_executable(BasketoGameEngine ${SOURCE_FILES})

if(LUA_FOUND)
    target_include_directories(BasketoGameEngine PUBLIC ${LUA_INCLUDE_DIR})
endif()

target_link_libraries(BasketoGameEngine
    PRIVATE 
    ${SDL2_LIBRARIES}
    ${SDL2_IMAGE_LIBRARIES}
    ${SDL2_TTF_LIBRARIES}
    ${SDL2_MIXER_LIBRARIES}
    ${OpenGL_LIBRARIES}
    ${LUA_LIBRARIES}
    PUBLIC sol2 
)

target_link_libraries(PhysicsTest
    ${SDL2_LIBRARIES}
)


add_custom_command(TARGET BasketoGameEngine POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "C:/Libraries/SDL2/SDL2.dll"
        "C:/Libraries/SDL2_image/SDL2_image.dll"
        "C:/Libraries/SDL2_ttf/SDL2_ttf.dll"
        "C:/Libraries/SDL2_mixer/SDL2_mixer.dll"
        "C:/Libraries/Lua/lua54.dll"
        $<TARGET_FILE_DIR:BasketoGameEngine>
)