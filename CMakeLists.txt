cmake_minimum_required(VERSION 3.5)

project(npc-generator VERSION "0.1" LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# -- Added by myself --
set(CMAKE_PREFIX_PATH "E:/Qt/6.2.4/mingw_64")

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets)
# -- Added by myself --
find_package(Qt6 REQUIRED COMPONENTS Xml Sql)

set(PROJECT_SOURCES
        # -- frontend section --
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui
        resources.qrc

        src/widgets/attributewidget.h src/widgets/attributewidget.cpp
        src/widgets/traitwidget.h src/widgets/traitwidget.cpp

        # -- helper classes --
        src/serializable.h src/serializable.cpp
        src/memio.h src/memio.cpp
        src/data/DataManager.h src/data/DataManager.cpp
        src/data/uniqueid.h src/data/uniqueid.cpp

        # -- edgesandhindrances class --
        src/generator/edgesandhindrances.h src/generator/edgesandhindrances.cpp
        src/generator/trait.h src/generator/trait.cpp

        # -- tiles priority generator --
        src/generator/tilemanager.h src/generator/tilemanager.cpp
        src/generator/tiles.h src/generator/tiles.cpp
        src/generator/tile.h src/generator/tile.cpp
        src/generator/tileschances.h src/generator/tileschances.cpp

        # -- generator namespace --
        src/generator/dice.h src/generator/dice.cpp
        src/generator/rolldices.h src/generator/rolldices.cpp
        src/generator/card.h src/generator/card.cpp

        # -- character class --
        src/generator/character.h src/generator/character.cpp
        src/generator/dices.h src/generator/dices.cpp
        src/generator/attributes.h src/generator/attributes.cpp
        src/generator/attribute.h src/generator/attribute.cpp
        src/generator/overview.h src/generator/overview.cpp
        src/generator/abilities.h src/generator/abilities.cpp
        src/generator/ability.h src/generator/ability.cpp

        # -- sessions system
        src/sessions/sessionmanager.h src/sessions/sessionmanager.cpp
        src/sessions/sessioncharacter.cpp src/sessions/sessioncharacter.h

        # -- tests --
        # tests/rolldicestest.h tests/rolldicestest.cpp
    )

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(npc-generator
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET npc-generator APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(npc-generator SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(npc-generator
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(npc-generator PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)
# -- Added by myself --
target_link_libraries(npc-generator PRIVATE Qt6::Xml Qt6::Sql)
target_link_libraries(npc-generator ${Boost_LIBRARIES})

set_target_properties(npc-generator PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(npc-generator)
endif()
