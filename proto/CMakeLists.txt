## Protobuf
set(protobuf_descriptors
  api.proto
  common.proto
  directions.proto
  info.proto
  options.proto
  sign.proto
  trip.proto
  transit.proto
  transit_fetch.proto
  incidents.proto
  status.proto)

if(ENABLE_DATA_TOOLS)
  # Only mjolnir needs the OSM PBF descriptors
  list(APPEND protobuf_descriptors
    ${VALHALLA_SOURCE_DIR}/third_party/OSM-binary/src/fileformat.proto
    ${VALHALLA_SOURCE_DIR}/third_party/OSM-binary/src/osmformat.proto)
endif()

#protobuf_generate_cpp(protobuf_srcs protobuf_hdrs ${protobuf_descriptors})

add_custom_command(
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/api.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/common.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/directions.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/info.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/options.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/sign.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/trip.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/transit.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/transit_fetch.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/incidents.pb.cc
  OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/status.pb.cc
  COMMAND ${CMAKE_CROSSCOMPILING_EMULATOR} $<TARGET_FILE:protoc>
    --cpp_out=${CMAKE_CURRENT_BINARY_DIR}
    --proto_path=${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/api.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/common.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/directions.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/info.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/options.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/sign.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/trip.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/transit.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/transit_fetch.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/incidents.proto
    ${CMAKE_CURRENT_SOURCE_DIR}/status.proto
  DEPENDS protoc
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/api.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/common.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/directions.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/info.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/options.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/sign.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/trip.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/transit.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/transit_fetch.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/incidents.proto
  DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/status.proto
  COMMENT "Generating Protobuf C++ Files"
)

add_library(valhalla-proto STATIC
    ${CMAKE_CURRENT_BINARY_DIR}/api.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/common.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/directions.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/info.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/options.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/sign.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/trip.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/transit.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/transit_fetch.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/incidents.pb.cc
    ${CMAKE_CURRENT_BINARY_DIR}/status.pb.cc
)
add_library(valhalla::proto ALIAS valhalla-proto)
target_include_directories(valhalla-proto PUBLIC ${CMAKE_CURRENT_BINARY_DIR})
target_link_libraries(valhalla-proto PUBLIC protobuf::libprotobuf)