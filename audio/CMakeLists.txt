# The audio/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_audio OBJECT
  audio_level.cc
  audio_receive_stream.cc
  audio_send_stream.cc
  audio_state.cc
  audio_transport_impl.cc
  channel_receive.cc
  channel_receive_frame_transformer_delegate.cc
  channel_send.cc
  channel_send_frame_transformer_delegate.cc
  nack_tracker.cc
  remix_resample.cc
  utility/audio_frame_operations.cc
  utility/channel_mixer.cc
  utility/channel_mixing_matrix.cc
)
target_link_libraries(webrtc_audio PRIVATE webrtc_config)
