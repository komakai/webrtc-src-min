# The modules/utility/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_utility OBJECT
  source/helpers_android.cc
  source/jvm_android.cc
)
target_link_libraries(webrtc_modules_utility PRIVATE webrtc_config)
