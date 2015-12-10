cmake_minimum_required(VERSION 2.8)

project("GPMap")


# find point cloud library
find_package(PCL 1.7.0 REQUIRED)
if(PCL_FOUND)
	message(STATUS "PCL INCLUDE DIRS ${PCL_INCLUDE_DIRS}")
	message(STATUS "PCL LIBS REQUIRED ${PCL_LIBRARIES}")
	include_directories(${PCL_INCLUDE_DIRS})
	link_directories(${PCL_LIBRARY_DIRS})
	add_definitions(${PCL_DEFINITIONS})
endif()


# find octomap
find_package(octomap REQUIRED)
if(octomap_FOUND)
	message(STATUS "OCTOMAP INCLUDE DIRS ${OCTOMAP_INCLUDE_DIRS}")
	message(STATUS "OCTOMAP LIBS REQUIRED ${OCTOMAP_LIBRARIES}")
	include_directories(BEFORE ${OCTOMAP_INCLUDE_DIRS})
	link_directories(${OCTOMAP_LIBRARY_DIRS})
	link_libraries(${OCTOMAP_LIBRARIES})
endif()

# find vtk
find_package(VTK REQUIRED)
if(VTK_FOUND)
	message(STATUS "VTK INCLUDE DIRS ${VTK_INCLUDE_DIRS}")
	message(STATUS "VTK LIBS REQUIRED ${VTK_LIBRARIES}")
	include_directories(${VTK_INCLUDE_DIRS})
	link_directories(${VTK_LIBRARY_DIRS})
	link_libraries(${VTK_LIBRARIES})
endif()

# find eigen 3
find_package( Eigen3 REQUIRED )
if(EIGEN3_FOUND)
	message(STATUS "EIGEN3 INCLUDE DIRS ${EIGEN3_INCLUDE_DIR}")
	include_directories( EIGEN3_INCLUDE_DIR )
endif()

# find BOOST
find_package(Boost 1.52 COMPONENTS date_time filesystem thread)
if(BOOST_FOUND)
	message(STATUS "BOOST INCLUDE DIRS ${Boost_INCLUDE_DIR}")
	message(STATUS "BOOST LIBS REQUIRED ${Boost_LIBRARIES}")
	include_directories(${Boost_INCLUDE_DIRS})
endif()

set(OpenGP_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/OpenGP)
message(STATUS "OPENGP INCLUDE DIRS ${OpenGP_INCLUDE}")
include_directories(${OpenGP_INCLUDE})


set(BCM_HEADERS 
	${CMAKE_CURRENT_SOURCE_DIR}/include/bcm/bcm.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/bcm/bcm_serialization.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/bcm/bcm_w_prior.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/bcm/gaussian.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/bcm/gaussian_serialization.hpp
)

set(COMMON_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/common/common.hpp
)

set(DATA_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/data/test_data.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/data/training_data.hpp
)

set(FEATURES_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/features/remove_NAN.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/features/surface_normal.hpp
)

set(FILTER_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/filter/filters.hpp
)

set(GPMAP_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/data_partitioning.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/macro_gpmap_basics.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/macro_gpmap.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/octree_container.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/octree_gpmap.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/gpmap/octree_viewer.hpp
)

set(IO_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/io/io.hpp
)

set(ISO_SURFACE_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/iso_surface/iso_surface.hpp
)

set(OCTOMAP_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/octomap/octomap.hpp
)

set(PLSC_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/plsc/plsc.hpp
)

set(SERIALIZATION_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/serialization/eigen_dense_base_addons.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/serialization/eigen_serialization.hpp
)

set(UTIL_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/util/color_map.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/util/data_types.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/util/filesystem.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/util/random.hpp
	${CMAKE_CURRENT_SOURCE_DIR}/include/util/timer.hpp
)

set(VISUALIZATION_HEADERS
	${CMAKE_CURRENT_SOURCE_DIR}/include/visualization/cloud_viewer.hpp
)


add_executable(bunny_gpmap 
main/bunny/main_bunny_gpmap.cpp
${SERIALIZATION_HEADERS} ${GPMAP_HEADERS} ${COMMON_HEADERS} ${IO_HEADERS} ${UTIL_HEADERS} ${BCM_HEADERS} ${VISUALIZATION_HEADERS} ${ISO_SURFACE_HEADERS} ${OCTOMAP_HEADERS} ${PLSC_HEADERS} ${FEATURES_HEADERS} ${DATA_HEADERS}
)

link_libraries(bunny_gpmap ${PCL_LIBRARIES} ${OCTOMAP_LIBRARIES} ${VTK_LIBRARIES} ${Boost_LIBRARIES})

