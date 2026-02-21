cmake_minimum_required(VERSION 3.10)
project(DubDubEngine)
set(CMAKE_CXX_STANDARD 20)

# Говорим CMake, что в папке libs/glfw-3.4 есть ещё один CMake проект
add_subdirectory(libs/glfw-3.4)

# GLAD - просто добавляем его .c файл к проекту
add_executable(DubDubEngine
        main.cpp
        libs/glad/src/glad.c
)

# Указываем пути к заголовкам
target_include_directories(DubDubEngine PRIVATE
        libs/glfw-3.4/include
        libs/glad/include
        libs/glm
)

# Линкуем (связываем) твою программу с библиотекой GLFW
target_link_libraries(DubDubEngine glfw)

# Копируем шейдеры после сборки
add_custom_command(TARGET DubDubEngine POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy
        ${CMAKE_SOURCE_DIR}/vertex_shader.glsl
        ${CMAKE_SOURCE_DIR}/fragment_shader.glsl
        $<TARGET_FILE_DIR:DubDubEngine>
        COMMENT "Copying shader files..."
)
