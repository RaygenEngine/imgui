project (ImGui)

set(SOURCE_FILES
	imgui.cpp
	imgui_demo.cpp
	imgui_draw.cpp
	imgui_widgets.cpp
    misc/cpp/imgui_stdlib.cpp
	examples/imgui_impl_glfw.cpp
	examples/imgui_impl_dx12.cpp
)
set(HEADER_FILES
	imconfig.h
	imextras.h
	imgui.h
	imgui_internal.h
    misc/cpp/imgui_stdlib.h
	examples/imgui_impl_glfw.h
	examples/imgui_impl_dx12.h
)

find_package(Vulkan REQUIRED)


add_library(ImGui ${SOURCE_FILES} ${HEADER_FILES})

target_include_directories(ImGui PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(ImGui PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/examples)
target_include_directories(ImGui PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/misc/cpp)


target_link_libraries(ImGui
	Vulkan::Vulkan
	glfw
	${GLFW_LIBRARIES}
)


if(MSVC)
	target_compile_options(ImGui PRIVATE "/MP")
endif()

# enforce cpp20
set_property(TARGET ImGui PROPERTY CXX_STANDARD 20)
set_property(TARGET ImGui PROPERTY CXX_STANDARD_REQUIRED ON)

source_group(TREE ${CMAKE_CURRENT_SOURCE_DIR} PREFIX ${PROJECT_NAME} FILES ${HEADER_FILES} ${SOURCE_FILES})
