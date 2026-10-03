# The sdk/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_sdk OBJECT
  android/native_api/jni/application_context_provider.cc
  android/native_api/jni/class_loader.cc
  android/native_api/jni/java_types.cc
  android/native_api/jni/jvm.cc
  android/native_api/stacktrace/stacktrace.cc
  android/src/jni/android_histogram.cc
  android/src/jni/android_metrics.cc
  android/src/jni/android_network_monitor.cc
  android/src/jni/android_video_track_source.cc
  android/src/jni/audio_device/audio_device_module.cc
  android/src/jni/audio_device/audio_record_jni.cc
  android/src/jni/audio_device/audio_track_jni.cc
  android/src/jni/audio_device/java_audio_device_module.cc
  android/src/jni/builtin_audio_decoder_factory_factory.cc
  android/src/jni/builtin_audio_encoder_factory_factory.cc
  android/src/jni/egl_base_10_impl.cc
  android/src/jni/encoded_image.cc
  android/src/jni/environment.cc
  android/src/jni/h264_utils.cc
  android/src/jni/java_i420_buffer.cc
  android/src/jni/jni_common.cc
  android/src/jni/jni_helpers.cc
  android/src/jni/jvm.cc
  android/src/jni/logging/log_sink.cc
  android/src/jni/native_capturer_observer.cc
  android/src/jni/nv12_buffer.cc
  android/src/jni/nv21_buffer.cc
  android/src/jni/pc/add_ice_candidate_observer.cc
  android/src/jni/pc/audio_track.cc
  android/src/jni/pc/call_session_file_rotating_log_sink.cc
  android/src/jni/pc/crypto_options.cc
  android/src/jni/pc/data_channel.cc
  android/src/jni/pc/dtmf_sender.cc
  android/src/jni/pc/ice_candidate.cc
  android/src/jni/pc/logging.cc
  android/src/jni/pc/media_constraints.cc
  android/src/jni/pc/media_source.cc
  android/src/jni/pc/media_stream.cc
  android/src/jni/pc/media_stream_track.cc
  android/src/jni/pc/owned_factory_and_threads.cc
  android/src/jni/pc/peer_connection.cc
  android/src/jni/pc/peer_connection_factory.cc
  android/src/jni/pc/rtc_certificate.cc
  android/src/jni/pc/rtc_stats_collector_callback_wrapper.cc
  android/src/jni/pc/rtp_capabilities.cc
  android/src/jni/pc/rtp_parameters.cc
  android/src/jni/pc/rtp_receiver.cc
  android/src/jni/pc/rtp_sender.cc
  android/src/jni/pc/rtp_transceiver.cc
  android/src/jni/pc/sdp_observer.cc
  android/src/jni/pc/session_description.cc
  android/src/jni/pc/ssl_certificate_verifier_wrapper.cc
  android/src/jni/pc/stats_observer.cc
  android/src/jni/pc/turn_customizer.cc
  android/src/jni/pc/video.cc
  android/src/jni/scoped_java_ref_counted.cc
  android/src/jni/software_video_decoder_factory.cc
  android/src/jni/software_video_encoder_factory.cc
  android/src/jni/timestamp_aligner.cc
  android/src/jni/video_codec_info.cc
  android/src/jni/video_codec_status.cc
  android/src/jni/video_decoder_factory_wrapper.cc
  android/src/jni/video_decoder_fallback.cc
  android/src/jni/video_decoder_wrapper.cc
  android/src/jni/video_encoder_factory_wrapper.cc
  android/src/jni/video_encoder_fallback.cc
  android/src/jni/video_encoder_wrapper.cc
  android/src/jni/video_frame.cc
  android/src/jni/video_sink.cc
  android/src/jni/video_track.cc
  android/src/jni/wrapped_native_i420_buffer.cc
  android/src/jni/yuv_helper.cc
  media_constraints.cc
)
target_link_libraries(webrtc_sdk PRIVATE webrtc_config boringssl jni_zero libyuv)

# libjingle_peerconnection_so.so, linked like gn does: all of WebRTC's
# objects, with unused code dropped by --gc-sections and only the JNI
# entry points exported.
add_library(jingle_peerconnection_so SHARED
  android/src/jni/jni_onload.cc
)
set_target_properties(jingle_peerconnection_so PROPERTIES
  LINK_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/android/android_only_jni_exports.lst)
target_link_libraries(jingle_peerconnection_so PRIVATE
  webrtc_config
  webrtc_api
  webrtc_audio
  webrtc_call
  webrtc_common_audio
  webrtc_common_video
  webrtc_logging
  webrtc_media
  webrtc_modules_async_audio_processing
  webrtc_modules_audio_coding
  webrtc_modules_audio_device
  webrtc_modules_audio_mixer
  webrtc_modules_audio_processing
  webrtc_modules_congestion_controller
  webrtc_modules_pacing
  webrtc_modules_remote_bitrate_estimator
  webrtc_modules_rtp_rtcp
  webrtc_modules_sframe
  webrtc_modules_third_party
  webrtc_modules_utility
  webrtc_modules_video_capture
  webrtc_modules_video_coding
  webrtc_net
  webrtc_p2p
  webrtc_pc
  webrtc_rtc_base
  webrtc_sdk
  webrtc_system_wrappers
  webrtc_video
  absl boringssl jni_zero libsrtp libyuv ndk_compat opus pffft rnnoise sframe
  EGL GLESv2 log dl m atomic
)
target_link_options(jingle_peerconnection_so PRIVATE
  -Wl,--version-script=${CMAKE_CURRENT_SOURCE_DIR}/android/android_only_jni_exports.lst
  -Wl,--gc-sections -Wl,--icf=all -Wl,-z,defs -Wl,--as-needed
  -Wl,-z,noexecstack -Wl,-z,relro -Wl,-z,now -Wl,--build-id
)
