# The system_wrappers/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_system_wrappers OBJECT
  source/clock.cc
  source/metrics.cc
)
target_link_libraries(webrtc_system_wrappers PRIVATE webrtc_config)
