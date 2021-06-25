cmake_minimum_required(VERSION 3.5)

project(qpcx)

if (MSVC)
    if(NOT QT_QMAKE_EXECUTABLE)
        find_program(QT_QMAKE_EXECUTABLE NAMES "qmake" "qmake-qt5" "qmake.exe")
    endif()
    if(NOT QT_QMAKE_EXECUTABLE)
        message(FATAL_ERROR "you have to set the path to the Qt5 qmake executable in CMakeUserPaths.cmake")
    endif()
        
    message(STATUS "QMake found: ${QT_QMAKE_EXECUTABLE}")
    get_filename_component(QT_QMAKE_PATH ${QT_QMAKE_EXECUTABLE} PATH)
        
    set(QT_ROOT ${QT_QMAKE_PATH}/)
    set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} ${QT_QMAKE_PATH}/../lib/cmake/Qt5)

endif()

find_package(Qt5 COMPONENTS Core Gui REQUIRED)
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

file(GLOB LIBQPCX_SOURCES "*.cpp")
file(GLOB LIBQPCX_HEADERS "*.h")

SET(CMAKE_DEBUG_POSTFIX "d")

add_library(${PROJECT_NAME} MODULE ${LIBQPCX_SOURCES} ${LIBQPCX_HEADERS})
target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Gui)

set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_BINARY_DIR}/libs)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_CURRENT_BINARY_DIR}/libs)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_REALLYRELEASE ${CMAKE_CURRENT_BINARY_DIR}/libs)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/imageformats")

# install to local plugins dir
set(plugins_dir "${CMAKE_INSTALL_PREFIX}")

# status
message(STATUS "")
message(STATUS "-----------------------------------------------------------------------------")
message(STATUS " ${PROJECT_NAME} configured")
message(STATUS " https://github.com/roniemartinez/libqpcx")
message(STATUS " it will be installed to ${plugins_dir}/imageformats")
message(STATUS "-----------------------------------------------------------------------------")

install(
  TARGETS ${PROJECT_NAME}
  LIBRARY DESTINATION "${plugins_dir}/imageformats")
