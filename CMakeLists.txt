cmake_minimum_required(VERSION 3.21)

project(ez-openssl)
include(FetchContent)

add_library(ez-openssl INTERFACE)

if (APPLE)
  FetchContent_Declare(
    ossl
    URL https://github.com/rleathart/ez-openssl/releases/download/v1.0.0/ez-openssl-v1.0.0-darwin-universal.tar.gz
    URL_HASH SHA512=896f9413143181cc0f16172e03da24aa0be033dba27e01a000471a2ea18b8a631e5dad73604d5dce44c78b2daa3e644a760d23ae3254610b3c7b1414cbc616c1
    DOWNLOAD_EXTRACT_TIMESTAMP ON
    )
  FetchContent_MakeAvailable(ossl)

  target_include_directories(ez-openssl INTERFACE "${ossl_SOURCE_DIR}/include/darwin")
  target_link_libraries(ez-openssl INTERFACE
    "${ossl_SOURCE_DIR}/lib/darwin/universal/libcrypto.a"
    "${ossl_SOURCE_DIR}/lib/darwin/universal/libssl.a"
    )
elseif (WIN32)
  FetchContent_Declare(
    ossl
    URL https://github.com/rleathart/ez-openssl/releases/download/v1.0.0/ez-openssl-v1.0.0-win32-amd64.tar.gz
    URL_HASH SHA512=7e10daf576ffbc7eba02bf94edf9df61604b09409a62934bc0274e738ce976b98c67ef31553b042c71008c35993065f6727bb606717c9b72ffba68c0b7c8a96d
    DOWNLOAD_EXTRACT_TIMESTAMP ON
    )
  FetchContent_MakeAvailable(ossl)

  target_include_directories(ez-openssl INTERFACE "${ossl_SOURCE_DIR}/include/win32")
  target_link_libraries(ez-openssl INTERFACE
    "${ossl_SOURCE_DIR}/lib/win32/amd64/libcrypto.lib"
    "${ossl_SOURCE_DIR}/lib/win32/amd64/libssl.lib"
    "Crypt32.lib"
    )
else()
  message(FATAL_ERROR "${PROJECT_NAME} does not support your platform :(")
endif()

add_library(ez-openssl::openssl ALIAS ez-openssl)
