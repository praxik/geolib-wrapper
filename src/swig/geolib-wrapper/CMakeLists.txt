set_source_files_properties( ${MODULE_FILE} PROPERTIES CPLUSPLUS ON )

if( CSHARP_FOUND OR CSHARP_MONO_FOUND )
  set( MODULE_NAME geolibwrapper )
  set( TARGET_NAME ${MODULE_NAME}_csharp )
  set( SWIG_LANGUAGE CSHARP )

  set( CMAKE_SWIG_FLAGS ${CMAKE_SWIG_GLOBAL_FLAGS} -namespace geolibwrapper -dllimport ${TARGET_NAME} )
  set( CMAKE_SWIG_OUTDIR ${CMAKE_CURRENT_BINARY_DIR}/csharp )
  set( SWIG_MODULE_${TARGET_NAME}_EXTRA_DEPS
    std_csharp )
  swig_add_library( ${TARGET_NAME} LANGUAGE csharp SOURCES ${MODULE_FILE} )
  swig_link_libraries( ${TARGET_NAME}
    geolibwrapper
    ${GeographicLib_LIBRARIES} )
  set_property( TARGET ${TARGET_NAME} PROPERTY PREFIX "${CMAKE_SHARED_LIBRARY_PREFIX}" )
  set_target_properties( ${TARGET_NAME} PROPERTIES
    BUILD_RPATH_USE_ORIGIN ON
    BUILD_RPATH "$ORIGIN"
    INSTALL_RPATH "$ORIGIN" )

  include( ModuleInstall )
endif()

if( PYTHONLIBS_FOUND )
  set( MODULE_NAME geolibwrapper )
  set( TARGET_NAME ${MODULE_NAME}_python )
  set( SWIG_LANGUAGE PYTHON )

  #Prevent UseSWIG from rebuilding source...
  set_property( SOURCE ${MODULE_FILE} PROPERTY SWIG_MODULE_NAME ${MODULE_NAME} )
  set( CMAKE_SWIG_FLAGS ${CMAKE_SWIG_GLOBAL_FLAGS} -interface _${TARGET_NAME} )
  set( CMAKE_SWIG_OUTDIR ${CMAKE_CURRENT_BINARY_DIR}/python )
  set( SWIG_MODULE_${TARGET_NAME}_EXTRA_DEPS
    _std_python )
  swig_add_library( ${TARGET_NAME} LANGUAGE python SOURCES ${MODULE_FILE} )
  swig_link_libraries( ${TARGET_NAME}
    geolibwrapper
    ${GeographicLib_LIBRARIES}
    ${PYTHON_LIBRARIES} )

  include( ModuleInstall )
endif()

if( RUBY_FOUND )
  set( MODULE_NAME geolibwrapper )
  set( TARGET_NAME ${MODULE_NAME} )
  set( SWIG_LANGUAGE RUBY )

  set( CMAKE_SWIG_FLAGS ${CMAKE_SWIG_GLOBAL_FLAGS} -c++ )
  set( CMAKE_SWIG_OUTDIR ${CMAKE_CURRENT_BINARY_DIR}/ruby )
  set( SWIG_MODULE_${TARGET_NAME}_EXTRA_DEPS
    std_ruby )
  swig_add_library( ${TARGET_NAME} LANGUAGE ruby SOURCES ${MODULE_FILE} )
  swig_link_libraries( ${TARGET_NAME}
    geolibwrapper
    ${GeographicLib_LIBRARIES}
    ${RUBY_LIBRARY} )

  include( ModuleInstall )
endif()
