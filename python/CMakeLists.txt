if(PYTHON_BINDING_CUSTOM_INSTALL_DIR)
  set(PYTHON_BINDING_INSTALL_DIR ${PYTHON_BINDING_CUSTOM_INSTALL_DIR})
  message(
    STATUS
    "[${PROJECT_NAME}] Using custom python binding install dir: ${PYTHON_BINDING_INSTALL_DIR}"
  )
else()
  set(PYTHON_BINDING_INSTALL_DIR "${Python_SITELIB}")
  message(
    STATUS
    "[${PROJECT_NAME}] Using default python binding install dir: ${PYTHON_BINDING_INSTALL_DIR}"
  )
endif()

boostpy_add_module(eigenpy_pywrap bindings.cpp)
target_link_libraries(eigenpy_pywrap PRIVATE eigenpy)
xxx_target_set_default_compile_options(eigenpy_pywrap PRIVATE)

set_target_properties(
  eigenpy_pywrap
  PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy
)

xxx_python_generate_init_py(
  eigenpy_pywrap OUTPUT_PATH
  ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy/__init__.py
)

boostpy_add_stubs(
  eigenpy_pywrap_stubs
  MODULE eigenpy.eigenpy_pywrap
  OUTPUT ${CMAKE_BINARY_DIR}/lib/site-packages
  PYTHON_PATH ${CMAKE_BINARY_DIR}/lib/site-packages
  DEPENDS eigenpy_pywrap
  VERBOSE
)

xxx_python_compile_all(DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy)

install(
  DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy
  DESTINATION ${PYTHON_BINDING_INSTALL_DIR}
  FILES_MATCHING
  PATTERN "*.py"
  PATTERN "*.pyc"
  PATTERN "*.typed"
  PATTERN "*.so"
  PATTERN "*.pyd"
)
