cmake_minimum_required(VERSION 3.1)

project(SortFilterProxyModelCmake VERSION 1.0 LANGUAGES CXX C) 

# Tell CMake to call moc automaticly
set(CMAKE_AUTOMOC ON)



set(PROJECT_VERSION_MAJOR "0")
set(PROJECT_VERSION_MINOR "0")
set(PROJECT_VERSION_PATCH "2")

set(PROJECT_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")


include(FeatureSummary)
include(GNUInstallDirs)

find_package(Qt5 COMPONENTS Core)

# Build SortFilterProxy as PlaceIndepndet Code here 
add_compile_options(-fPIC) 
add_subdirectory(3rdParty/SortFilterProxyModel)

#Build Shared lib with Object here 
add_library( SortFilterProxyModelCmake
    SHARED
    $<TARGET_OBJECTS:SortFilterProxyModel> 
    )

#set target Version
set_target_properties(SortFilterProxyModelCmake PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(SortFilterProxyModelCmake PROPERTIES SOVERSION ${PROJECT_VERSION_MAJOR})




# install library
install(TARGETS SortFilterProxyModelCmake
    EXPORT SortFilterProxyModelCmakeExport
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    )



# configure *ConfigVersion.cmake
include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    SortFilterProxyModelCmakeConfigVersion.cmake
    VERSION ${PACKAGE_VERSION}
    COMPATIBILITY SameMajorVersion
    )


# configure *Config.cmake
configure_file(SortFilterProxyModelCmakeConfig.cmake.in SortFilterProxyModelCmakeConfig.cmake @ONLY)


# install *Config(Version).cmake
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/SortFilterProxyModelCmakeConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/SortFilterProxyModelCmakeConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/SortFilterProxyModelCmake
    )

# install targets cmake-files
install(EXPORT SortFilterProxyModelCmakeExport
    FILE SortFilterProxyModelCmakeTargets.cmake
    NAMESPACE ZeraQml::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/SortFilterProxyModelCmake
    )

feature_summary(WHAT ALL FATAL_ON_MISSING_REQUIRED_PACKAGES)
