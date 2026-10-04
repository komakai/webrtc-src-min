# The common_video/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_common_video ${WEBRTC_LIBRARY_TYPE}
  bitrate_adjuster.cc
  frame_rate_estimator.cc
  framerate_controller.cc
  generic_frame_descriptor/generic_frame_info.cc
  h264/h264_bitstream_parser.cc
  h264/h264_common.cc
  h264/pps_parser.cc
  h264/sps_parser.cc
  h264/sps_vui_rewriter.cc
  libyuv/webrtc_libyuv.cc
  video_frame_buffer.cc
  video_frame_buffer_pool.cc
)
target_link_libraries(webrtc_common_video PRIVATE webrtc_config libyuv)
