cmake_minimum_required (VERSION 3.0)
project(Amnesia)

file(GLOB all_sources RELATIVE ${CMAKE_CURRENT_SOURCE_DIR}
    *.cpp
    *.h
)

set(compile_defs "")

option(USE_GAMEPAD "Enable gamepad support" ON)
if(USE_GAMEPAD)
	list(APPEND compile_defs USE_GAMEPAD)
endif()

add_executable(Amnesia MACOSX_BUNDLE
    ${all_sources}
)

target_link_libraries(Amnesia HPL2)
set_target_properties(Amnesia PROPERTIES
  COMPILE_DEFINITIONS "${compile_defs}"
  MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/Game-Info.plist"
  MACOSX_BUNDLE_ICON_FILE "Lux.icns"
  MACOSX_BUNDLE_BUNDLE_VERSION ${VERSION}
  MACOSX_BUNDLE_SHORT_VERSION_STRING ${VERSION}
  MACOSX_BUNDLE_GUI_IDENTIFIER "com.frictionalgames.Amnesia"
  MACOSX_BUNDLE_DISPLAY_NAME "Amnesia"
  MACOSX_BUNDLE_BUNDLE_NAME "Amnesia: The Dark Descent"
)

IF(APPLE)
add_definitions(
    -DMAC_OS
)
ELSEIF(LINUX)
add_definitions(
    -DLINUX
)
ENDIF()

IF(APPLE)
  PostBuildMacBundle(Amnesia "${HPL2_FRAMEWORKS}" "${HPL2_LIBRARIES}")
  install(TARGETS Amnesia
    RUNTIME DESTINATION "${CMAKE_CURRENT_SOURCE_DIR}/../../redist"
    BUNDLE DESTINATION "${CMAKE_CURRENT_SOURCE_DIR}/../../redist"
    )
ENDIF()
