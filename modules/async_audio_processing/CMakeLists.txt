# The modules/async_audio_processing/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_async_audio_processing ${WEBRTC_LIBRARY_TYPE}
  async_audio_processing.cc
)
target_link_libraries(webrtc_modules_async_audio_processing PRIVATE webrtc_config)
