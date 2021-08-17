#######################################
# Vicon Subscriber

add_executable(imgui_viewer_app
src/main.cpp
)

target_link_libraries(imgui_viewer_app
imgui
)

target_include_directories(imgui_viewer_app PUBLIC include)
