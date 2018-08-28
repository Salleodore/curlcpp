IF(BIICODE)
    INCLUDE(biicode/cmake/tools)

    ACTIVATE_CPP11()
    ADD_BIICODE_TARGETS()
ELSE()
    cmake_minimum_required(VERSION 2.8)
    #if using an older VERSION of curl ocsp stapling can be disabled
    #set(CURL_MIN_VERSION "7.61.0-DEV")
    # Setting up project
    project(CURLCPP)


    # Add MacPorts
    if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
      set(CMAKE_PREFIX_PATH /opt/local ${CMAKE_PREFIX_PATH})
    endif()

	set(CURL_LIBRARY libcurl)
	
    # Checking for dependencies
    find_package(CURL ${CURL_MIN_VERSION} REQUIRED)

	
    # Set up include pathes
    include_directories(${CURLCPP_SOURCE_DIR}/include
		${CURL_INCLUDE_DIRS})
	
    file(GLOB HEADER_LIST "${CMAKE_SOURCE_DIR}/include/*.h")
    install (FILES ${HEADER_LIST} DESTINATION include)

    # Set up source directories
    add_subdirectory(src)

    set(CURLCPP_LIB_LOCATION "$<TARGET_FILE:${curlcpp}>")

    configure_file(CMake/curlcppConfig.cmake.in
      ${CURLCPP_BINARY_DIR}/curlcppConfig.cmake @ONLY)
ENDIF()
