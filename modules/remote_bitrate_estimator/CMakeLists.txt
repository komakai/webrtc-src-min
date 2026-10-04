# The modules/remote_bitrate_estimator/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_remote_bitrate_estimator ${WEBRTC_LIBRARY_TYPE}
  aimd_rate_control.cc
  bwe_defines.cc
  congestion_control_feedback_generator.cc
  congestion_control_feedback_tracker.cc
  inter_arrival.cc
  overuse_detector.cc
  overuse_estimator.cc
  packet_arrival_map.cc
  remote_bitrate_estimator_abs_send_time.cc
  remote_bitrate_estimator_single_stream.cc
  transport_sequence_number_feedback_generator.cc
)
target_link_libraries(webrtc_modules_remote_bitrate_estimator PRIVATE webrtc_config)
