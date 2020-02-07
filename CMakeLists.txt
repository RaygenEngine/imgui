project (ImGui)

set(SOURCE_FILES
	imgui.cpp
	imgui_demo.cpp
	imgui_draw.cpp
	imgui_widgets.cpp
	examples/imgui_impl_vulkan.cpp
	examples/imgui_impl_glfw.cpp
	examples/imgui_impl_win32.cpp
	examples/example_glfw_vulkan/main.cpp
)
set(HEADER_FILES
	imconfig.h
	imgui.h
	imgui_internal.h
	examples/imgui_impl_vulkan.h
	examples/imgui_impl_glfw.h
	examples/imgui_impl_win32.h
)

find_package(Vulkan REQUIRED)


add_library(ImGui ${SOURCE_FILES} ${HEADER_FILES})

target_include_directories(ImGui PRIVATE ${CMAKE_CURRENT_SOURCE_DIR})
target_include_directories(ImGui PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/examples)

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
