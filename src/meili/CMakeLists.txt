file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/meili/*.h)

set(sources
  topk_search.cc
  routing.cc
  geometry_helpers.cc
  map_matcher_factory.cc
  config.cc)

set(sources_with_warnings
  candidate_search.cc
  map_matcher.cc
  match_route.cc
  transition_cost_model.cc
  viterbi_search.cc)

valhalla_module(NAME meili
  SOURCES ${sources}
  SOURCES_WITH_WARNINGS ${sources_with_warnings}
  HEADERS ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla
      ${VALHALLA_SOURCE_DIR}/third_party/date/include
    PRIVATE
      ${CMAKE_BINARY_DIR}
  DEPENDS
    valhalla::sif
    ${valhalla_protobuf_targets}
    boost
    rapidjson
    $<$<BOOL:${WIN32}>:dirent>)
