set(_public_cxx_features
	cxx_std_11
	cxx_strong_enums
	cxx_defaulted_functions
	cxx_defaulted_move_initializers
	cxx_explicit_conversions
)
set(_private_cxx_features
	cxx_thread_local
	cxx_nullptr
	cxx_constexpr
	cxx_variadic_templates
	cxx_alignas
	cxx_override
	cxx_static_assert
	cxx_deleted_functions
)

install(
	FILES
		${CMAKE_SOURCE_DIR}/include/NanoLog.hpp
	DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
install(
	FILES
		${CMAKE_CURRENT_SOURCE_DIR}/NanoLog.cpp
	DESTINATION src/${PROJECT_NAME}
)

add_library(NanoLog NanoLog.cpp)
target_compile_features(NanoLog PUBLIC ${_public_cxx_features})
target_compile_features(NanoLog PRIVATE ${_private_cxx_features})
#set_target_properties(NanoLog PROPERTIES
#	CXX_EXTENSIONS OFF
#)
target_include_directories(NanoLog PUBLIC
	$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
install(
	TARGETS NanoLog
	EXPORT ${PROJECT_NAME}-config
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

add_library(NanoLog_src INTERFACE)
target_sources(NanoLog_src INTERFACE
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/NanoLog.cpp>
	$<INSTALL_INTERFACE:src/${PROJECT_NAME}/NanoLog.cpp>
)
target_compile_features(NanoLog_src INTERFACE ${_public_cxx_features})
target_compile_features(NanoLog_src INTERFACE ${_private_cxx_features})
target_include_directories(NanoLog_src INTERFACE
	$<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)
install(
	TARGETS NanoLog_src
	EXPORT ${PROJECT_NAME}-config
	RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
	ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
	LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(EXPORT ${PROJECT_NAME}-config DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME} NAMESPACE ${PROJECT_NAME}::)