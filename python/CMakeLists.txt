xxx_boostpy_add_module(eigenpy_pywrap bindings.cpp)
target_link_libraries(eigenpy_pywrap PRIVATE eigenpy)
xxx_target_set_default_compile_options(eigenpy_pywrap PRIVATE)
xxx_target_set_output_directory(eigenpy_pywrap OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy)

xxx_python_generate_init_py(
  eigenpy_pywrap OUTPUT_PATH
  ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy/__init__.py
)

xxx_boostpy_add_stubs(
  eigenpy_pywrap_stubs
  MODULE eigenpy.eigenpy_pywrap
  OUTPUT ${CMAKE_BINARY_DIR}/lib/site-packages
  PYTHON_PATH ${CMAKE_BINARY_DIR}/lib/site-packages
  DEPENDS eigenpy_pywrap
  VERBOSE
)

xxx_python_compile_all(DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy)

xxx_python_compute_install_dir(python_install_dir)
install(
  DIRECTORY ${CMAKE_BINARY_DIR}/lib/site-packages/eigenpy
  DESTINATION ${python_install_dir}
  FILES_MATCHING
  PATTERN "*.py"
  PATTERN "*.pyc"
  PATTERN "*.typed"
  PATTERN "*.so"
  PATTERN "*.pyd"
)
