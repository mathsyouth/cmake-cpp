cmake_minimum_required(VERSION 3.8.2)
project(Foo VERSION 1.0.0 LANGUAGES CXX)

file(GLOB _HDRS "include/foo/*.hpp" "include/foo/*.hxx")
file(GLOB _SRCS "src/*.[hc]pp")

add_library(${PROJECT_NAME} ${_HDRS} ${_SRCS})
target_include_directories(${PROJECT_NAME} PUBLIC
	$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
	$<INSTALL_INTERFACE:include>)
set_target_properties(${PROJECT_NAME} PROPERTIES
	PUBLIC_HEADER ${_HDRS}
	VERSION ${PROJECT_VERSION})
#target_link_libraries(${PROJECT_NAME} PUBLIC ...)
add_library(CMakeCpp::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

# Test
include(CTest)
if(BUILD_TESTING)
	add_subdirectory(test)
endif()

# Install
include(GNUInstallDirs)
install(TARGETS ${PROJECT_NAME}
	EXPORT CMakeCppTargets
	PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/foo
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
	#RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	)
