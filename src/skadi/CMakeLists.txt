file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/skadi/*.h)

valhalla_module(NAME skadi
  SOURCES
    sample.cc
    util.cc
  HEADERS
    ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${VALHALLA_SOURCE_DIR}
      ${VALHALLA_SOURCE_DIR}/valhalla

    PRIVATE
      ${VALHALLA_SOURCE_DIR}/third_party/lz4/lib
  DEPENDS
    valhalla::baldr
    boost
    zlibstatic
    lz4_static
    $<$<BOOL:${WIN32}>:dirent>)
