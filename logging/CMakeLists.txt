# The logging/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_logging OBJECT
  rtc_event_log/encoder/bit_writer.cc
  rtc_event_log/encoder/rtc_event_log_encoder_common.cc
  rtc_event_log/encoder/var_int.cc
  rtc_event_log/events/fixed_length_encoding_parameters_v3.cc
  rtc_event_log/events/rtc_event_alr_state.cc
  rtc_event_log/events/rtc_event_audio_network_adaptation.cc
  rtc_event_log/events/rtc_event_audio_playout.cc
  rtc_event_log/events/rtc_event_audio_receive_stream_config.cc
  rtc_event_log/events/rtc_event_audio_send_stream_config.cc
  rtc_event_log/events/rtc_event_bwe_update_delay_based.cc
  rtc_event_log/events/rtc_event_bwe_update_loss_based.cc
  rtc_event_log/events/rtc_event_bwe_update_scream.cc
  rtc_event_log/events/rtc_event_dtls_transport_state.cc
  rtc_event_log/events/rtc_event_dtls_writable_state.cc
  rtc_event_log/events/rtc_event_field_encoding.cc
  rtc_event_log/events/rtc_event_field_encoding_parser.cc
  rtc_event_log/events/rtc_event_field_extraction.cc
  rtc_event_log/events/rtc_event_ice_candidate_pair.cc
  rtc_event_log/events/rtc_event_ice_candidate_pair_config.cc
  rtc_event_log/events/rtc_event_neteq_set_minimum_delay.cc
  rtc_event_log/events/rtc_event_probe_cluster_created.cc
  rtc_event_log/events/rtc_event_probe_result_failure.cc
  rtc_event_log/events/rtc_event_probe_result_success.cc
  rtc_event_log/events/rtc_event_route_change.cc
  rtc_event_log/events/rtc_event_rtcp_packet_incoming.cc
  rtc_event_log/events/rtc_event_rtcp_packet_outgoing.cc
  rtc_event_log/events/rtc_event_rtp_packet_incoming.cc
  rtc_event_log/events/rtc_event_rtp_packet_outgoing.cc
  rtc_event_log/events/rtc_event_video_receive_stream_config.cc
  rtc_event_log/events/rtc_event_video_send_stream_config.cc
  rtc_event_log/ice_logger.cc
  rtc_event_log/rtc_stream_config.cc
)
target_link_libraries(webrtc_logging PRIVATE webrtc_config)
