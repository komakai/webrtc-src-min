# The modules/congestion_controller/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_congestion_controller ${WEBRTC_LIBRARY_TYPE}
  ect1_policy.cc
  goog_cc/acknowledged_bitrate_estimator.cc
  goog_cc/acknowledged_bitrate_estimator_interface.cc
  goog_cc/alr_detector.cc
  goog_cc/bitrate_estimator.cc
  goog_cc/congestion_window_pushback_controller.cc
  goog_cc/delay_based_bwe.cc
  goog_cc/goog_cc_network_control.cc
  goog_cc/inter_arrival_delta.cc
  goog_cc/link_capacity_estimator.cc
  goog_cc/loss_based_bwe_v2.cc
  goog_cc/probe_bitrate_estimator.cc
  goog_cc/probe_controller.cc
  goog_cc/robust_throughput_estimator.cc
  goog_cc/send_side_bandwidth_estimation.cc
  goog_cc/trendline_estimator.cc
  receive_side_congestion_controller.cc
  remb_throttler.cc
  rtp/control_handler.cc
  rtp/transport_feedback_adapter.cc
  rtp/transport_feedback_demuxer.cc
  scream/delay_based_congestion_control.cc
  scream/loss_estimator.cc
  scream/scream_feedback.cc
  scream/scream_network_controller.cc
  scream/scream_v2.cc
  scream/scream_v2_parameters.cc
)
target_link_libraries(webrtc_modules_congestion_controller PRIVATE webrtc_config)
