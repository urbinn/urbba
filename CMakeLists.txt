CMAKE_MINIMUM_REQUIRED(VERSION 2.9)

# Compiler
SET(CMAKE_CXX_STANDARD 11)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)

SET(LIBRARY_NAME "urbg2o")
SET(SOURCES "src/motion-only-ba.cpp" "src/bind.cpp" "src/local-ba.cpp")

LIST(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake_modules)

IF(APPLE)
    SET(CMAKE_SHARED_LIBRARY_SUFFIX ".so")
    SET(CMAKE_MACOSX_RPATH ON)
ENDIF(APPLE)

FIND_PACKAGE(PythonLibs 3 REQUIRED)
FIND_PACKAGE(g2o REQUIRED)
FIND_PACKAGE(Eigen3 REQUIRED)
FIND_PACKAGE(CSparse REQUIRED)
FIND_PACKAGE(Cholmod REQUIRED)
find_package(OpenCV 3 REQUIRED)

ADD_LIBRARY(${LIBRARY_NAME} SHARED ${SOURCES})
SET_TARGET_PROPERTIES(${LIBRARY_NAME} PROPERTIES PREFIX "")

message("PYTHON_INCLUDE_DIRS " ${PYTHON_INCLUDE_DIRS})
message("OPENCV_INCLUDE_DIRS " ${OPENCV_INCLUDE_DIRS})
message("PYTHON_LIBRARIES " ${PYTHON_LIBRARIES})
message("OPENCV_LIBS " ${OPENCV_LIBS})

INCLUDE_DIRECTORIES(
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${G2O_INCLUDE_DIR}
    ${CSPARSE_INCLUDE_DIR}
    ${Cholmod_INCLUDE_DIR}
    ${EIGEN3_INCLUDE_DIR}
    ${PYTHON_INCLUDE_DIRS}
    "/opt/jupyterhub/anaconda/include/python3.6m"
    "/opt/jupyterhub/anaconda/lib/python3.6/site-packages/numpy/core/include"
)

LIST(APPEND G2O_LIBS
    cxsparse
    cholmod
    g2o_cli g2o_ext_freeglut_minimal g2o_simulator
    g2o_solver_slam2d_linear g2o_types_icp g2o_types_slam2d
    g2o_core g2o_interface g2o_solver_csparse g2o_solver_structure_only
    g2o_types_sba g2o_types_slam3d g2o_csparse_extension
    g2o_opengl_helper g2o_solver_dense g2o_stuff
    g2o_types_sclam2d g2o_parser g2o_solver_pcg
    g2o_types_data g2o_types_sim3
)

target_link_libraries(
    ${LIBRARY_NAME}
    ${G2O_LIBS}
    ${PYTHON_LIBRARIES}
    ${OpenCV_LIBS}
)
