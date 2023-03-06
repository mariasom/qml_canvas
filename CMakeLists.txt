cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

project(QmlCanvasApp
    DESCRIPTION "Qml Canvas App"
    LANGUAGES CXX
)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# manually setting qt6 paths on tesla server -> not good!
set(QT_VERSION_MAJOR 6)
set(QT_VERSION 6.4.0)
set(QT_COMPILER "msvc2019_64")

if(APPLE)
    set(Qt${QT_VERSION_MAJOR}_DIR "/Users/mariasom/Qt/6.4.2/macos/lib/cmake/Qt${QT_VERSION_MAJOR}")
elseif(WIN32)
    set(Qt${QT_VERSION_MAJOR}_DIR "C:/Qt/${QT_VERSION}/${QT_COMPILER}/lib/cmake/Qt${QT_VERSION_MAJOR}")
    set(Qt${QT_VERSION_MAJOR}WidgetsTools_DIR "C:/Qt/${QT_VERSION}/${QT_COMPILER}/lib/cmake/Qt${QT_VERSION_MAJOR}WidgetsTools")
    set(Qt${QT_VERSION_MAJOR}CoreTools_DIR "C:/Qt/${QT_VERSION}/${QT_COMPILER}/lib/cmake/Qt${QT_VERSION_MAJOR}CoreTools")
    set(Qt${QT_VERSION_MAJOR}GuiTools_DIR "C:/Qt/${QT_VERSION}/${QT_COMPILER}/lib/cmake/Qt${QT_VERSION_MAJOR}GuiTools")
endif()

# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

# Find the Qt libraries for Qt Quick/QML
find_package(Qt${QT_VERSION_MAJOR} ${QT_VERSION} REQUIRED Core Qml Quick Gui Widgets QuickWidgets)

# add the qml.qrc file
qt_add_resources(qml_QRC src/qml.qrc)

# add source files
set(SOURCE 
	${SOURCE}
	src/main.cpp
)

# Tell CMake to create the project executable
qt_add_executable(${PROJECT_NAME} ${SOURCE} ${qml_QRC})

set_target_properties(${PROJECT_NAME} PROPERTIES
    WIN32_EXECUTABLE TRUE
    MACOSX_BUNDLE TRUE
)

# Use the Qml/Quick modules from Qt 6
target_link_libraries(${PROJECT_NAME} PUBLIC Qt${QT_VERSION_MAJOR}::Core Qt${QT_VERSION_MAJOR}::Qml Qt${QT_VERSION_MAJOR}::Quick Qt${QT_VERSION_MAJOR}::Gui Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::QuickWidgets)

qt_add_qml_module(${PROJECT_NAME}
    URI ${PROJECT_NAME}
    VERSION 1.0
    QML_FILES src/main.qml)

set(QML_IMPORT_PATH ${PROJECT_SOURCE_DIR} CACHE STRING "QML Import Paths")

if(APPLE)
    install(TARGETS ${PROJECT_NAME}
        RUNTIME DESTINATION "${PROJECT_BINARY_DIR}"
        BUNDLE DESTINATION "${PROJECT_BINARY_DIR}"
        LIBRARY DESTINATION "${PROJECT_BINARY_DIR}"
    )
elseif(WIN32)
    add_custom_command(TARGET ${PROJECT_NAME}
                POST_BUILD COMMAND "C:/Qt/${QT_VERSION}/${QT_COMPILER}/bin/windeployqt.exe" "${PROJECT_BINARY_DIR}"
                COMMENT "windeployqt executed...")
endif()

