#
# Copyright (c) 2014-2019 CNRS Copyright (c) 2018-2024 INRIA
#

add_custom_target(eigenpy_tests)
add_dependencies(eigenpy_tests eigenpy)

add_test(
  NAME "Eigenpy: Python: import eigenpy"
  COMMAND
    ${CMAKE_COMMAND} -E env PYTHONPATH=${CMAKE_BINARY_DIR}/lib/site-packages
    $<TARGET_FILE:Python::Interpreter> -c "import eigenpy"
  WORKING_DIRECTORY ${CMAKE_BINARY_DIR}
)

function(eigenpy_add_test name)
  boostpy_add_module(${name} ${name}.cpp)
  target_link_libraries(${name} PRIVATE eigenpy)
  xxx_target_set_default_compile_options(${name} PRIVATE)
  add_dependencies(eigenpy_tests ${name})

  set_target_properties(
    ${name}
    PROPERTIES LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${name}
  )

  xxx_python_generate_init_py(
      ${name}
      OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/${name}/__init__.py
  )

  add_test(
    NAME "Eigenpy Python: import ${name}"
    COMMAND
      ${CMAKE_COMMAND} -E env PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}
      $<TARGET_FILE:Python::Interpreter> -c "import ${name}"
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  )
endfunction()

eigenpy_add_test(matrix)
eigenpy_add_test(type_info)
eigenpy_add_test(multiple_registration)
if(BUILD_TESTING_SCIPY)
  eigenpy_add_test(sparse_matrix)
endif()
eigenpy_add_test(tensor)
eigenpy_add_test(geometry)
eigenpy_add_test(complex)
eigenpy_add_test(deprecation_policy)
eigenpy_add_test(return_by_ref)
eigenpy_add_test(include)
eigenpy_add_test(eigen_ref)
eigenpy_add_test(user_type)
eigenpy_add_test(std_vector)
eigenpy_add_test(std_array)
eigenpy_add_test(std_pair)
eigenpy_add_test(std_map)
eigenpy_add_test(user_struct)
eigenpy_add_test(std_unique_ptr)
eigenpy_add_test(virtual_factory)

set(TEST_TYPE "boost::optional")
set(MODNAME boost_optional)
configure_file(optional.cpp.in ${CMAKE_CURRENT_SOURCE_DIR}/boost_optional.cpp)
eigenpy_add_test(boost_optional)

set(TEST_TYPE "std::optional")
set(MODNAME std_optional)
configure_file(optional.cpp.in ${CMAKE_CURRENT_SOURCE_DIR}/std_optional.cpp)
eigenpy_add_test(std_optional)

set(TEST_TYPE "boost::variant")
set(MODNAME boost_variant)
configure_file(variant.cpp.in ${CMAKE_CURRENT_SOURCE_DIR}/boost_variant.cpp)
eigenpy_add_test(boost_variant)

set(TEST_TYPE "std::variant")
set(MODNAME std_variant)
configure_file(variant.cpp.in ${CMAKE_CURRENT_SOURCE_DIR}/std_variant.cpp)
eigenpy_add_test(std_variant)

function(eigenpy_python_add_test name source)
  set(test_name "Eigenpy: Python ${name}")
  add_test(
    NAME ${test_name}
    COMMAND $<TARGET_FILE:Python::Interpreter> ${CMAKE_CURRENT_SOURCE_DIR}/${source}
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
  )
  set_tests_properties(
    ${test_name}
    PROPERTIES
      ENVIRONMENT "PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}:${CMAKE_BINARY_DIR}/lib/site-packages"
      DEPENDS eigenpy
  )
endfunction()

eigenpy_python_add_test("py-matrix" "python/test_matrix.py")
eigenpy_python_add_test("py-type-info" "python/test_type_info.py")
eigenpy_python_add_test("py-multiple-registration" "python/test_multiple_registration.py")
eigenpy_python_add_test("py-tensor" "python/test_tensor.py")
eigenpy_python_add_test("py-Geometry" "python/test_Geometry.py")
eigenpy_python_add_test("py-complex" "python/test_complex.py")
eigenpy_python_add_test("py-deprecation-policy" "python/test_deprecation_policy.py")
eigenpy_python_add_test("py-return-by-ref" "python/test_return_by_ref.py")
eigenpy_python_add_test("py-eigen-ref" "python/test_eigen_ref.py")
eigenpy_python_add_test("py-user-type" "python/test_user_type.py")

eigenpy_python_add_test("py-dimensions" "python/test_dimensions.py")
eigenpy_python_add_test("py-version" "python/test_version.py")
eigenpy_python_add_test("py-EigenSolver" "python/test_EigenSolver.py")
eigenpy_python_add_test("py-GeneralizedEigenSolver" "python/test_GeneralizedEigenSolver.py")
eigenpy_python_add_test(
  "py-GeneralizedSelfAdjointEigenSolver"
  "python/test_GeneralizedSelfAdjointEigenSolver.py"
)
eigenpy_python_add_test("py-ComplexEigenSolver" "python/test_ComplexEigenSolver.py")
eigenpy_python_add_test("py-ComplexSchur" "python/test_ComplexSchur.py")
eigenpy_python_add_test("py-SelfAdjointEigenSolver" "python/test_SelfAdjointEigenSolver.py")
eigenpy_python_add_test("py-HessenbergDecomposition" "python/test_HessenbergDecomposition.py")

eigenpy_python_add_test("py-RealQZ" "python/test_RealQZ.py")

eigenpy_python_add_test("py-RealSchur" "python/test_RealSchur.py")

eigenpy_python_add_test("py-Tridiagonalization" "python/test_Tridiagonalization.py")

eigenpy_python_add_test("py-LLT" "python/test_LLT.py")

eigenpy_python_add_test("py-LDLT" "python/test_LDLT.py")

eigenpy_python_add_test("py-FullPivLU" "python/test_FullPivLU.py")

eigenpy_python_add_test("py-PartialPivLU" "python/test_PartialPivLU.py")

eigenpy_python_add_test("py-id" "python/test_id.py")

eigenpy_python_add_test("py-QR" "python/test_QR.py")

eigenpy_python_add_test("py-BDCSVD" "python/test_BDCSVD.py")

eigenpy_python_add_test("py-JacobiSVD" "python/test_JacobiSVD.py")

if(NOT WIN32)
  eigenpy_python_add_test("py-MINRES" "python/test_MINRES.py")
endif()

eigenpy_python_add_test("py-std-vector" "python/test_std_vector.py")

eigenpy_python_add_test("py-std-array" "python/test_std_array.py")

eigenpy_python_add_test("py-std-map" "python/test_std_map.py")

eigenpy_python_add_test("py-std-pair" "python/test_std_pair.py")

eigenpy_python_add_test("py-user-struct" "python/test_user_struct.py")

eigenpy_python_add_test("py-std-unique-ptr" "python/test_std_unique_ptr.py")

eigenpy_python_add_test("py-virtual-factory" "python/test_virtual_factory.py")

set(MODNAME boost_optional)
configure_file(python/test_optional.py.in python/test_${MODNAME}.py)
eigenpy_python_add_test(${MODNAME} "python/test_${MODNAME}.py")

set(MODNAME std_optional)
configure_file(python/test_optional.py.in python/test_${MODNAME}.py)
eigenpy_python_add_test(${MODNAME} "python/test_${MODNAME}.py")

set(MODNAME boost_variant)
configure_file(python/test_variant.py.in python/test_${MODNAME}.py)
eigenpy_python_add_test(${MODNAME} "python/test_${MODNAME}.py")

set(MODNAME std_variant)
configure_file(python/test_variant.py.in python/test_${MODNAME}.py)
eigenpy_python_add_test(${MODNAME} "python/test_${MODNAME}.py")

if(BUILD_TESTING_SCIPY)
  eigenpy_python_add_test("py-sparse-matrix" "python/test_sparse_matrix.py")

  eigenpy_python_add_test("py-SimplicialLLT" "python/decompositions/sparse/test_SimplicialLLT.py")

  eigenpy_python_add_test("py-SimplicialLDLT" "python/decompositions/sparse/test_SimplicialLDLT.py")

  eigenpy_python_add_test("py-IncompleteCholesky" "python/test_IncompleteCholesky.py")

  eigenpy_python_add_test("py-IncompleteLUT" "python/test_IncompleteLUT.py")

  eigenpy_python_add_test("py-SparseLU" "python/decompositions/sparse/test_SparseLU.py")

  eigenpy_python_add_test("py-SparseQR" "python/decompositions/sparse/test_SparseQR.py")

  if(BUILD_WITH_CHOLMOD_SUPPORT)
    eigenpy_python_add_test(
      "py-CholmodSimplicialLLT"
      "python/decompositions/sparse/cholmod/test_CholmodSimplicialLLT.py"
    )

    eigenpy_python_add_test(
      "py-CholmodSimplicialLDLT"
      "python/decompositions/sparse/cholmod/test_CholmodSimplicialLDLT.py"
    )

    eigenpy_python_add_test(
      "py-CholmodSupernodalLLT"
      "python/decompositions/sparse/cholmod/test_CholmodSupernodalLLT.py"
    )
  endif()

  if(BUILD_WITH_ACCELERATE_SUPPORT)
    eigenpy_python_add_test("py-Accelerate" "python/decompositions/sparse/test_Accelerate.py")
  endif()
endif()
