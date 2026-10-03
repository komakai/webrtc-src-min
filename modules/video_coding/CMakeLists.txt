# The modules/video_coding/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_video_coding OBJECT
  chain_diff_calculator.cc
  codecs/av1/av1_svc_config.cc
  codecs/vp8/vp8_scalability.cc
  codecs/vp9/svc_config.cc
  decoder_database.cc
  encoded_frame.cc
  fec_controller_default.cc
  frame_dependencies_calculator.cc
  frame_helpers.cc
  generic_decoder.cc
  h264_sprop_parameter_sets.cc
  h264_sps_pps_tracker.cc
  h26x_packet_buffer.cc
  histogram.cc
  include/video_codec_interface.cc
  include/video_error_codes_utils.cc
  loss_notification_controller.cc
  media_opt_util.cc
  nack_requester.cc
  packet_buffer.cc
  rtp_frame_id_only_ref_finder.cc
  rtp_frame_reference_finder.cc
  rtp_generic_ref_finder.cc
  rtp_seq_num_only_ref_finder.cc
  rtp_vp8_ref_finder.cc
  rtp_vp9_ref_finder.cc
  svc/create_scalability_structure.cc
  svc/scalability_mode_util.cc
  svc/scalability_structure_full_svc.cc
  svc/scalability_structure_key_svc.cc
  svc/scalability_structure_l2t2_key_shift.cc
  svc/scalability_structure_simulcast.cc
  svc/scalable_video_controller_no_layering.cc
  svc/svc_rate_allocator.cc
  timing/decode_time_percentile_filter.cc
  timing/timing.cc
  utility/bandwidth_quality_scaler.cc
  utility/decoded_frames_history.cc
  utility/frame_dropper.cc
  utility/framerate_controller_deprecated.cc
  utility/ivf_file_reader.cc
  utility/ivf_file_writer.cc
  utility/qp_parser.cc
  utility/quality_scaler.cc
  utility/reference_buffer_tracker.cc
  utility/simulcast_rate_allocator.cc
  utility/simulcast_utility.cc
  utility/temporal_layer_rate_tracker.cc
  utility/vp8_header_parser.cc
  utility/vp9_uncompressed_header_parser.cc
  video_codec_initializer.cc
  video_coding_defines.cc
  video_receiver2.cc
)
target_link_libraries(webrtc_modules_video_coding PRIVATE webrtc_config)

if(APPLE)
  target_sources(webrtc_modules_video_coding PRIVATE
    codecs/h264/h264.cc
    codecs/h264/h264_color_space.cc
  )
endif()
