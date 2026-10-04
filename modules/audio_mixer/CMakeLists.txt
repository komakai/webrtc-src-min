# The modules/audio_mixer/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_audio_mixer ${WEBRTC_LIBRARY_TYPE}
  audio_frame_manipulator.cc
  audio_mixer_impl.cc
  default_output_rate_calculator.cc
  frame_combiner.cc
)
target_link_libraries(webrtc_modules_audio_mixer PRIVATE webrtc_config)
