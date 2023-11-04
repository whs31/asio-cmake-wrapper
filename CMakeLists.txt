cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

project(ASIO
  VERSION 1.28.0
  DESCRIPTION "C++ library for asynchronous I/O"
  HOMEPAGE_URL "https://github.com/chriskohlhoff/asio"
  LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Threads REQUIRED)

add_library(asio INTERFACE)
target_include_directories(asio SYSTEM INTERFACE ${CMAKE_SOURCE_DIR}/libs/asio/include)
message(STATUS "[ASIO] Path to ASIO: ${CMAKE_SOURCE_DIR}/libs/asio/include")
target_compile_definitions(asio INTERFACE ASIO_STANDALONE ASIO_NO_DEPRECATED)
target_link_libraries(asio INTERFACE Threads::Threads)

if(WIN32)
  # macro see @ https://stackoverflow.com/a/40217291/1746503
  macro(get_win32_winnt version)
    if(CMAKE_SYSTEM_VERSION)
      set(ver ${CMAKE_SYSTEM_VERSION})
      string(REGEX MATCH "^([0-9]+).([0-9])" ver ${ver})
      string(REGEX MATCH "^([0-9]+)" verMajor ${ver})
      if("${verMajor}" MATCHES "10")
        set(verMajor "A")
        string(REGEX REPLACE "^([0-9]+)" ${verMajor} ver ${ver})
      endif("${verMajor}" MATCHES "10")
      string(REPLACE "." "" ver ${ver})
      string(REGEX REPLACE "([0-9A-Z])" "0\\1" ver ${ver})
      set(${version} "0x${ver}")
    endif()
  endmacro()

  if(NOT DEFINED _WIN32_WINNT)
    get_win32_winnt(ver)
    set(_WIN32_WINNT ${ver})
  endif()

  message(STATUS "[ASIO] Set _WIN32_WINNET=${_WIN32_WINNT}")

  target_compile_definitions(asio INTERFACE _WIN32_WINNT=${_WIN32_WINNT} WIN32_LEAN_AND_MEAN)
endif()
