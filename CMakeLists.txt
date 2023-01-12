cmake_minimum_required(VERSION 3.12 FATAL_ERROR)

project(QmlCanvasApp
    DESCRIPTION "Qml Canvas App"
    LANGUAGES CXX
)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)

# Find the Qt libraries for Qt Quick/QML
find_package(Qt5 REQUIRED Core Qml Quick Gui Widgets QuickWidgets)

# add the qml.qrc file
qt5_add_resources(qml_QRC src/qml.qrc)

# add source files
set(SOURCE 
	${SOURCE}
	src/main.cpp
)

# Tell CMake to create the helloworld executable
add_executable(${PROJECT_NAME} ${SOURCE} ${qml_QRC})

# Use the Qml/Quick modules from Qt 5.
target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Qml Qt5::Quick Qt5::Gui Qt5::Widgets Qt5::QuickWidgets )

set(QML_IMPORT_PATH ${PROJECT_SOURCE_DIR} CACHE STRING "QML Import Paths")