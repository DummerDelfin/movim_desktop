cmake_minimum_required(VERSION 2.8.11)

set(PROJECT_NAME "movim-desktop")
project(${PROJECT_NAME})

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

# Find the QtWidgets library
find_package(Qt5 CONFIG REQUIRED COMPONENTS
    Widgets
    Core
    Qml
    Quick
    WebEngine)

qt5_add_resources(QT_RESOURCES qml.qrc)
add_executable(${PROJECT_NAME} main.cpp ${QT_RESOURCES})

# Use the Widgets module from Qt 5.
target_link_libraries(${PROJECT_NAME} Qt5::Widgets Qt5::Core Qt5::Qml Qt5::Quick Qt5::WebEngine)

install(PROGRAMS "build/${PROJECT_NAME}" DESTINATION "/usr/bin/")
install(FILES "movim.desktop" DESTINATION "/usr/share/applications")
install(FILES "icons/22.png" DESTINATION "/usr/share/icons/hicolor/22x22/apps/" RENAME "movim.png")
install(FILES "icons/24.png" DESTINATION "/usr/share/icons/hicolor/24x24/apps/" RENAME "movim.png")
install(FILES "icons/32.png" DESTINATION "/usr/share/icons/hicolor/32x32/apps/" RENAME "movim.png")
install(FILES "icons/36.png" DESTINATION "/usr/share/icons/hicolor/36x36/apps/" RENAME "movim.png")
install(FILES "icons/42.png" DESTINATION "/usr/share/icons/hicolor/42x42/apps/" RENAME "movim.png")
install(FILES "icons/48.png" DESTINATION "/usr/share/icons/hicolor/48x48/apps/" RENAME "movim.png")
install(FILES "icons/64.png" DESTINATION "/usr/share/icons/hicolor/64x64/apps/" RENAME "movim.png")
install(FILES "icons/128.png" DESTINATION "/usr/share/icons/hicolor/128x128/apps/" RENAME "movim.png")
install(FILES "icons/256.png" DESTINATION "/usr/share/icons/hicolor/256x256/apps/" RENAME "movim.png")
install(FILES "icons/vectorial.svg" DESTINATION "/usr/share/icons/hicolor/scalable/apps/" RENAME "movim.svg")

# Find the architecture
IF(NOT CPACK_DEBIAN_PACKAGE_ARCHITECTURE)
  FIND_PROGRAM(DPKG_CMD dpkg)
  IF(NOT DPKG_CMD)
    MESSAGE(STATUS "Can not find dpkg in your path, default to i386.")
    SET(CPACK_DEBIAN_PACKAGE_ARCHITECTURE i386)
  ENDIF(NOT DPKG_CMD)
  EXECUTE_PROCESS(COMMAND "${DPKG_CMD}" --print-architecture
    OUTPUT_VARIABLE CPACK_DEBIAN_PACKAGE_ARCHITECTURE
    OUTPUT_STRIP_TRAILING_WHITESPACE
    )
ENDIF(NOT CPACK_DEBIAN_PACKAGE_ARCHITECTURE)

# CPack
set(CPACK_GENERATOR "DEB")
set(CPACK_DEBIAN_PACKAGE_DESCRIPTION "Desktop Client for Movim")
set(CPACK_SOURCE_GENERATOR "TBZ2")
set(CPACK_PACKAGE_VERSION_MAJOR "0")
set(CPACK_PACKAGE_VERSION_MINOR "14")
set(CPACK_PACKAGE_VERSION_PATCH "0")
set(CPACK_STRIP_FILES ON)
set(CPACK_PACKAGE_CONTACT "Timothée Jaussoin <edhelas@movim.eu>")
set(CPACK_DEBIAN_PACKAGE_NAME "${CMAKE_PROJECT_NAME}")
set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}_${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH}_${CPACK_DEBIAN_PACKAGE_ARCHITECTURE}")
set(CPACK_DEBIAN_PACKAGE_HOMEPAGE "https://movim.eu/")
set(CPACK_DEBIAN_PACKAGE_SHLIBDEPS ON)
include(CPack)
