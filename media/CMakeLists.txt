# The media/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_media OBJECT
  base/codec.cc
  base/codec_comparators.cc
  base/codec_list.cc
  base/media_channel_impl.cc
  base/media_constants.cc
  base/media_engine.cc
  base/rid_description.cc
  base/rtp_utils.cc
  base/sdp_video_format_utils.cc
  base/stream_params.cc
  base/turn_utils.cc
  engine/adm_helpers.cc
  engine/webrtc_media_engine.cc
  engine/webrtc_video_engine.cc
  engine/webrtc_voice_engine.cc
  sctp/dcsctp_transport.cc
  sctp/sctp_transport_factory.cc
)
target_link_libraries(webrtc_media PRIVATE webrtc_config)
