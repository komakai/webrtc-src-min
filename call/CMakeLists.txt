# The call/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_call OBJECT
  adaptation/adaptation_constraint.cc
  adaptation/broadcast_resource_listener.cc
  adaptation/degradation_preference_provider.cc
  adaptation/encoder_settings.cc
  adaptation/resource_adaptation_processor.cc
  adaptation/resource_adaptation_processor_interface.cc
  adaptation/video_source_restrictions.cc
  adaptation/video_stream_adapter.cc
  adaptation/video_stream_input_state.cc
  adaptation/video_stream_input_state_provider.cc
  audio_receive_stream.cc
  audio_send_stream.cc
  audio_state.cc
  bitrate_allocator.cc
  call.cc
  call_config.cc
  flexfec_receive_stream.cc
  flexfec_receive_stream_impl.cc
  payload_type_picker.cc
  rtp_bitrate_configurator.cc
  rtp_config.cc
  rtp_demuxer.cc
  rtp_payload_params.cc
  rtp_stream_receiver_controller.cc
  rtp_transport_controller_send.cc
  rtp_video_sender.cc
  rtx_receive_stream.cc
  syncable.cc
  version.cc
  video_receive_stream.cc
  video_send_stream.cc
)
target_link_libraries(webrtc_call PRIVATE webrtc_config)
