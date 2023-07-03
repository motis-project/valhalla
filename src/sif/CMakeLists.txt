file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/sif/*.h)

set(sources
  autocost.cc
  bicyclecost.cc
  hierarchylimits.cc
  motorcyclecost.cc
  motorscootercost.cc
  nocost.cc
  pedestriancost.cc
  transitcost.cc
  truckcost.cc
  dynamiccost.cc
  recost.cc)

valhalla_module(NAME sif
  SOURCES ${sources}
  HEADERS ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla
      ${VALHALLA_SOURCE_DIR}/third_party/date/include
    PRIVATE
      ${VALHALLA_SOURCE_DIR}/third_party/rapidjson/include
  DEPENDS
    valhalla::baldr
    valhalla::proto
    ${valhalla_protobuf_targets}
    boost
    rapidjson
    $<$<BOOL:${WIN32}>:dirent>)
