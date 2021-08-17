if(MSVC AND NOT "${MSVC_VERSION}" LESS 1400)
  add_definitions( "/MP" )
endif()

add_definitions(
  -DTW_STATIC
  -DTW_NO_LIB_PRAGMA
  -DTW_NO_DIRECT3D
  -DGLEW_STATIC
  -D_CRT_SECURE_NO_WARNINGS
)

### GLFW ###
add_subdirectory(glfw)

include_directories(
  glfw/include/GLFW/
  glew/include/
)

### GLEW ###
set(GLEW_SOURCE
  glew/src/glew.c
)
set(GLEW_HEADERS
)
add_library(glew STATIC
  ${GLEW_SOURCE}
  ${GLEW_INCLUDE}
)
target_link_libraries(glew
  ${OPENGL_LIBRARY}
)

### IMGUI ###

add_library(imgui STATIC
  imgui/imgui.cpp
  imgui/imgui_demo.cpp
  imgui/imgui_draw.cpp
  imgui/imgui_tables.cpp
  imgui/imgui_widgets.cpp
  imgui/backends/imgui_impl_glfw.cpp
  imgui/backends/imgui_impl_opengl3.cpp
)

target_include_directories(imgui PUBLIC 
imgui/backends
imgui
)

# Treat windows and ubuntu differently.
if(MSVC)
  target_link_libraries(imgui
    glew
    glfw
  )
endif(MSVC)
if(UNIX AND NOT APPLE)
  target_link_libraries(imgui
    glew
    glfw
    X11
    Xi
    Xrandr
    Xxf86vm
    Xinerama
    Xcursor
    rt
    m
    pthread
    dl
  )
endif(UNIX AND NOT APPLE)
if(APPLE)
  include_directories(/System/Library/Frameworks)
  find_library(COCOA_LIBRARY Cocoa)
  find_library(IOKIT_LIBRARY IOKit)
  find_library(COREVIDEO_LIBRARY CoreVideo)
  target_link_libraries(imgui
    glew
    glfw
    ${COCOA_LIBRARY}
    ${IOKIT_LIBRARY}
    ${COREVIDEO_LIBRARY}
  )
endif(APPLE)
