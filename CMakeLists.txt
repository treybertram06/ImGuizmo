# CMakeLists.txt for ImGuizmo (placed in vendor/imguizmo)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_library(imguizmo STATIC
        ImGuizmo.cpp
)

# Require C++20 (inherits from parent if already set, but this is safe)
target_compile_features(imguizmo PUBLIC cxx_std_20)

# ImGuizmo includes "ImGuizmo.h" and Dear ImGui headers ("imgui.h")
target_include_directories(imguizmo
        PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}
)

# Link with Dear ImGui target so its include dirs and settings propagate
target_link_libraries(imguizmo
        PUBLIC
        imgui
)

# Optional: ImGuizmo benefits from this define for math ops on ImVec types
target_compile_definitions(imguizmo
        PRIVATE
        IMGUI_DEFINE_MATH_OPERATORS
)

# Ensure this target never uses PCH, even if propagated via interface
set_target_properties(imguizmo PROPERTIES
        DISABLE_PRECOMPILE_HEADERS ON
)

# Also mark the source file to skip any PCH usage
set_source_files_properties(${CMAKE_CURRENT_LIST_DIR}/ImGuizmo.cpp
        PROPERTIES SKIP_PRECOMPILE_HEADERS ON
)
