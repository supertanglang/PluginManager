cmake_minimum_required (VERSION 2.6)

project (PluginManager)

# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)

# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

#set (CMAKE_PREFIX_PATH)
#message("CMAKE_PREFIX_PATH:" ${CMAKE_PREFIX_PATH} )

set (CMAKE_PREFIX_PATH "/home/nicolasrobert/Programming/Qt5.4.0/5.4/gcc_64/lib/cmake/" )

# Find the QtXml library
find_package(Qt5Xml)
find_package(Qt5Widgets)

#version number
set (PluginManager_VERSION_MAJOR 1)
set (PluginManager_VERSION_MINOR 0)


add_subdirectory(PluginManager)
add_subdirectory(Apps)
add_subdirectory(Examples)

#message( "PROJECT_SOURCE_DIR:" ${PROJECT_SOURCE_DIR} )

#define a macro to simplify adding tests, then use it
macro (do_simpletest name arg result)
  add_test (${name} ${PluginManager_BINARY_DIR}/PluginManager/Tests/PluginManagerTest ${arg})
  set_tests_properties (${name}
    PROPERTIES PASS_REGULAR_EXPRESSION ${result})
endmacro (do_simpletest)

enable_testing()

do_simpletest(FileDoesNotExist ${PluginManager_BINARY_DIR}/PluginManager/Tests/XXX.json "File does not exist")
do_simpletest(NoConfigurationTag ${PluginManager_BINARY_DIR}/PluginManager/Tests/noconfiguration.json "Missing json object attribute /configuration/")
do_simpletest(NoDescriptionTag ${PluginManager_BINARY_DIR}/PluginManager/Tests/nodescription.json "Missing or empty json string attribute /description/")
do_simpletest(NoAuthorTag ${PluginManager_BINARY_DIR}/PluginManager/Tests/noauthor.json "Missing or empty json string attribute /author/")
do_simpletest(NoPlugins ${PluginManager_BINARY_DIR}/PluginManager/Tests/noplugins.json "Missing or empty json array attribute /plugins/")
do_simpletest(InvalidPluginFile ${PluginManager_BINARY_DIR}/PluginManager/Tests/invalidpluginfile.json "The shared library was not found")
do_simpletest(NotEnoughExtensions ${PluginManager_BINARY_DIR}/PluginManager/Tests/notenoughextensions.json "This plugin configuration could not provide enough extensions of type -INumberGenerator-")
do_simpletest(TooManyExtensions ${PluginManager_BINARY_DIR}/PluginManager/Tests/toomanyextensions.json "This plugin configuration provided too many extensions of type -INumberGenerator-")