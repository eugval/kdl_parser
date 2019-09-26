cmake_minimum_required(VERSION 2.8.3)

project(kdl_parser_py)

find_package(catkin REQUIRED)

catkin_package(
  CATKIN_DEPENDS urdfdom_py
)

catkin_python_setup()

catkin_install_python(PROGRAMS ${PROJECT_NAME}/urdf.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

if(CATKIN_ENABLE_TESTING)
  find_package(catkin REQUIRED COMPONENTS rostest)
  add_rostest(test/test_kdl_parser.launch)

  # Force CMake to find python again in case it was called before to find python 2
  unset(PYTHON_EXECUTABLE CACHE)
  unset(PYTHONINTERP_FOUND CACHE)
  find_package(PythonInterp 3 REQUIRED)
  add_test(NAME test_py3_syntax COMMAND "${PYTHON_EXECUTABLE}" "-m" "flake8" "--select" "E999")
endif()
