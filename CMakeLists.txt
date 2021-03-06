cmake_minimum_required (VERSION 2.6)
project (client CXX)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DASIO_STANDALONE -pthread")

SET(ASIO_SUBMODULE_DIR "${PROJECT_SOURCE_DIR}/lib/asio/asio/include")

# detection stuff for ASIO
if (EXISTS "${ASIO_SUBMODULE_DIR}/asio.hpp")
    message("ASIO has been checked out as a git submodule: ${ASIO_SUBMODULE_DIR}")
  include_directories(${ASIO_SUBMODULE_DIR})
else()
  message("ASIO has NOT been checked out as a git submodule...")
  if (ASIO_HOME)
    message("ASIO_HOME defined in cache: ${ASIO_HOME}")
    include_directories(${ASIO_HOME})  
  else()
    if(DEFINED ENV{ASIO_HOME})
      message("ASIO_HOME defined in environment: $ENV{ASIO_HOME}")  
      include_directories($ENV{ASIO_HOME})    
    else()
      message("ASIO_HOME was not defined. ASIO expected to be on include path")  
    endif()
  endif()
endif()

find_package(OpenCV REQUIRED)

include_directories(include)

add_executable(server src/server.cpp src/udp_server.cpp src/webcam.cpp)
target_link_libraries(server ${ZLIB_LIBRARIES} ${OpenCV_LIBRARIES})

add_executable(client src/client.cpp src/udp_client.cpp src/webcam.cpp)
target_link_libraries(client ${ZLIB_LIBRARIES} ${OpenCV_LIBRARIES})
