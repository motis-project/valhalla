## Process the IANA Time Zone Database files
set(tz_db_files africa antarctica asia australasia backward etcetera europe
  pacificnew northamerica southamerica systemv leapseconds)

foreach(tz_db_file ${tz_db_files})
  add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/date_time_${tz_db_file}.h
    COMMAND ${CMAKE_COMMAND} -DMSVC=${MSVC} -P ${VALHALLA_SOURCE_DIR}/cmake/Binary2Header.cmake
        ${VALHALLA_SOURCE_DIR}/date_time/${tz_db_file}
        ${CMAKE_CURRENT_BINARY_DIR}/date_time_${tz_db_file}.h
        --variable-name date_time_${tz_db_file} --skip 1 --raw
    WORKING_DIRECTORY ${VALHALLA_SOURCE_DIR}
    COMMENT "Compiling date_time/${tz_db_file} to date_time_${tz_db_file}.h"
    DEPENDS ${VALHALLA_SOURCE_DIR}/date_time/${tz_db_file})
endforeach()

## Process the windowsZones.xml file
add_custom_command(OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/date_time_windows_zones.h
  COMMAND ${CMAKE_COMMAND} -DMSVC=${MSVC} -P ${VALHALLA_SOURCE_DIR}/cmake/Binary2Header.cmake
      ${VALHALLA_SOURCE_DIR}/date_time/windowsZones.xml
      ${CMAKE_CURRENT_BINARY_DIR}/date_time_windows_zones.h
      --variable-name date_time_windows_zones_xml --skip 1 --raw
  WORKING_DIRECTORY ${VALHALLA_SOURCE_DIR}
  COMMENT "Compiling date_time/windowsZones.xml to date_time_windows_zones.h"
  DEPENDS ${VALHALLA_SOURCE_DIR}/date_time/windowsZones.xml)

file(GLOB headers ${VALHALLA_SOURCE_DIR}/valhalla/baldr/*.h)

set(includes
    ${VALHALLA_SOURCE_DIR}
     ${VALHALLA_SOURCE_DIR}/valhalla
     ${CMAKE_CURRENT_BINARY_DIR}/src/baldr
     )

set(sources
    accessrestriction.cc
    admin.cc
    attributes_controller.cc
    compression_utils.cc
    connectivity_map.cc
    curler.cc
    datetime.cc
    directededge.cc
    edgeinfo.cc
    graphid.cc
    incident_singleton.h
    edgetracker.cc
    nodeinfo.cc
    location.cc
    merge.cc
    pathlocation.cc
    predictedspeeds.cc
    tilehierarchy.cc
    turn.cc
    shortcut_recovery.h
    streetname.cc
    streetnames.cc
    streetnames_factory.cc
    streetname_us.cc
    streetnames_us.cc
    transitdeparture.cc
    transitroute.cc
    transitschedule.cc
    transittransfer.cc
    laneconnectivity.cc
    verbal_text_formatter.cc
    verbal_text_formatter_us.cc
    verbal_text_formatter_us_co.cc
    verbal_text_formatter_us_tx.cc
    verbal_text_formatter_factory.cc)

set (sources_with_warnings
    graphreader.cc
    graphtile.cc
    graphtileheader.cc)

list(APPEND sources
    #basic timezone stuff
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_africa.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_antarctica.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_asia.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_australasia.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_backward.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_etcetera.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_europe.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_leapseconds.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_northamerica.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_pacificnew.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_southamerica.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_systemv.h
    ${CMAKE_CURRENT_BINARY_DIR}/date_time_windows_zones.h)

list(APPEND sources_with_warnings tz_alt.cpp)

valhalla_module(NAME baldr
  SOURCES
    ${sources}
  SOURCES_WITH_WARNINGS
    ${sources_with_warnings}

  HEADERS
    ${headers}
  INCLUDE_DIRECTORIES
    PUBLIC
      ${includes}
    PRIVATE
      ${CMAKE_CURRENT_BINARY_DIR}

  DEPENDS
    valhalla::midgard
    valhalla::proto
    ${valhalla_protobuf_targets}
    boost
    CURL::CURL
    zlibstatic
    date
    date-tz
    rapidjson)
