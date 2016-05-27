---
layout: post
title: "CMakeLists Examples"
category: programming
tags: [cmake, qt]
use_math: true
---

### CMakeLists Examples
Add `-DCMAKE_BUILD_TYPE=Debug` to debug the project in QT Creator.
![QtCreator-Setting](http://i.stack.imgur.com/dyYJn.png)

```cmake
cmake_minimum_required(VERSION 2.9)

set(TARGET_BUILD_PLATFORM x64)
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Add OpenCV:
set(OpenCV_STATIC OFF)
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})
link_directories(${OpenCV_LIBRARY_DIRS})
add_definitions(${OpenCV_DEFINITIONS})

project(ARViewer)

include_directories(./libs/QGLViewer/include/)
include_directories(./libs/libzxing/include/)
include_directories(./libs/utils/)

add_subdirectory(./libs/utils/)
add_subdirectory(./src/generalar/)

# Add QT: 
set(CMAKE_PREFIX_PATH "C:/Qt/Qt5.3.0/5.3/msvc2013_64_opengl/")
set(CMAKE_LIBRARY_PATH "C:/Program Files (x86)/Windows Kits/8.0/Lib/win8/um/x64")
set(QTBIN_PATH "C:/Qt/Qt5.3.0/5.3/msvc2013_64_opengl/bin/")
find_package(Qt5Widget)
find_package(Qt5Gui)
find_package(Qt5Xml)
find_package(Qt5OpenGL)

file(GLOB ARVIEWER_SRC
	./src/arviewer/*.h
	./src/arviewer/*.cpp
)
add_executable(ARViewer ${ARVIEWER_SRC})



if(MSVC AND OpenCV_STATIC)
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
endif()

target_link_libraries(ARViewer  
	Qt5::Widgets
	Qt5::Xml
	Qt5::Core
	Qt5::Gui
	Qt5::OpenGL
	${OpenCV_LIBS}
	debug ${CMAKE_CURRENT_SOURCE_DIR}/libs/libzxing/lib/${TARGET_BUILD_PLATFORM}/libzxingd.lib
	optimized ${CMAKE_CURRENT_SOURCE_DIR}/libs/libzxing/lib/${TARGET_BUILD_PLATFORM}/libzxing.lib
	${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/lib/${TARGET_BUILD_PLATFORM}/opengl32.lib
	${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/lib/${TARGET_BUILD_PLATFORM}/glut32.lib
	${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/lib/${TARGET_BUILD_PLATFORM}/glu32.lib
	${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/lib/${TARGET_BUILD_PLATFORM}/glew32.lib
	debug ${CMAKE_CURRENT_SOURCE_DIR}/libs/QGLViewer/lib/${TARGET_BUILD_PLATFORM}/QGLViewerd2.lib
	optimized ${CMAKE_CURRENT_SOURCE_DIR}/libs/QGLViewer/lib/${TARGET_BUILD_PLATFORM}/QGLViewer2.lib
	GeneralAR
	utils
)

set(BINARY_DEBUG_DIR ${PROJECT_BINARY_DIR}/Debug/)
set(BINARY_RELEASE_DIR ${PROJECT_BINARY_DIR}/Release/)
file(MAKE_DIRECTORY ${BINARY_DEBUG_DIR})
file(MAKE_DIRECTORY ${BINARY_RELEASE_DIR})

file(GLOB DLL_FILES_DEBUG
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/bin/${TARGET_BUILD_PLATFORM}/gl*.dll
	${PROJECT_BINARY_DIR}/libs/utils/Debug/*.dll
	${QTBIN_PATH}/icu*.dll
	${CMAKE_CURRENT_SOURCE_DIR}/libs/QGLViewer/bin/${TARGET_BUILD_PLATFORM}/*d2.dll
	${QTBIN_PATH}/Qt5Cored.dll
	${QTBIN_PATH}/Qt5Guid.dll
	${QTBIN_PATH}/Qt5OpenGLd.dll
	${QTBIN_PATH}/Qt5Widgetsd.dll
	${QTBIN_PATH}/Qt5Xmld.dll
)
file(GLOB DLL_FILES_RELEASE
    ${CMAKE_CURRENT_SOURCE_DIR}/libs/OpenGL/bin/${TARGET_BUILD_PLATFORM}/gl*.dll
	${PROJECT_BINARY_DIR}/libs/utils/Release/*.dll
	${QTBIN_PATH}/icu*.dll
	${CMAKE_CURRENT_SOURCE_DIR}/libs/QGLViewer/bin/${TARGET_BUILD_PLATFORM}/*[^d]2.dll
	${QTBIN_PATH}/Qt5Core.dll
	${QTBIN_PATH}/Qt5Gui.dll
	${QTBIN_PATH}/Qt5OpenGL.dll
	${QTBIN_PATH}/Qt5Widgets.dll
	${QTBIN_PATH}/Qt5Xml.dll
)
file(COPY ${DLL_FILES_DEBUG} DESTINATION ${BINARY_DEBUG_DIR})
file(COPY ${DLL_FILES_RELEASE} DESTINATION ${BINARY_RELEASE_DIR})

file (GLOB DATA_FILES
	${CMAKE_CURRENT_SOURCE_DIR}/data/*.*
)
file (COPY ${DATA_FILES} DESTINATION ${PROJECT_BINARY_DIR}/data/)

```