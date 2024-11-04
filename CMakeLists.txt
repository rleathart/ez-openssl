cmake_minimum_required(VERSION 3.21)

project(ez-openssl)
include(FetchContent)

add_library(ez-openssl INTERFACE)

if (APPLE)
  FetchContent_Declare(
    ez-openssl-ossl
    URL https://github.com/rleathart/ez-openssl/releases/download/v1.1.0/ez-openssl-v1.1.0-darwin-universal.tar.gz
    URL_HASH SHA512=74a53377ac69f5bf0ad02d09c809adfc09bd1e1c0f9f8fc0e85ffbc8e18fc5dd29a82230836246f5364bd60063110d1c48b9918cd07ba1edac858e05438b87c3
    DOWNLOAD_EXTRACT_TIMESTAMP ON
    )
  FetchContent_MakeAvailable(ez-openssl-ossl)

  target_include_directories(ez-openssl INTERFACE "${ez-openssl-ossl_SOURCE_DIR}/include/darwin")
  target_link_libraries(ez-openssl INTERFACE
    "${ez-openssl-ossl_SOURCE_DIR}/lib/darwin/universal/libcrypto.a"
    "${ez-openssl-ossl_SOURCE_DIR}/lib/darwin/universal/libssl.a"
    )
elseif (WIN32)
  FetchContent_Declare(
    ez-openssl-ossl
    URL https://github.com/rleathart/ez-openssl/releases/download/v1.1.0/ez-openssl-v1.1.0-win32-amd64.tar.gz
    URL_HASH SHA512=7e10daf576ffbc7eba02bf94edf9df61604b09409a62934bc0274e738ce976b98c67ef31553b042c71008c35993065f6727bb606717c9b72ffba68c0b7c8a96d
    DOWNLOAD_EXTRACT_TIMESTAMP ON
    )
  FetchContent_MakeAvailable(ez-openssl-ossl)

  target_include_directories(ez-openssl INTERFACE "${ez-openssl-ossl_SOURCE_DIR}/include/win32")
  target_link_libraries(ez-openssl INTERFACE
    "${ez-openssl-ossl_SOURCE_DIR}/lib/win32/amd64/libcrypto.lib"
    "${ez-openssl-ossl_SOURCE_DIR}/lib/win32/amd64/libssl.lib"
    "Crypt32.lib"
    )
else()
  message(FATAL_ERROR "${PROJECT_NAME} does not support your platform :(")
endif()

add_library(ez-openssl::openssl ALIAS ez-openssl)
