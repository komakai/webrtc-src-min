# The sdk/ sources of the gn targets libjingle_peerconnection_so (Android) or
# framework_objc (iOS, WebRTC.framework) depend on, and the library itself.
add_library(webrtc_sdk OBJECT
  media_constraints.cc
)
target_link_libraries(webrtc_sdk PRIVATE webrtc_config boringssl libyuv)

if(ANDROID)
  target_sources(webrtc_sdk PRIVATE
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
  )
  target_link_libraries(webrtc_sdk PRIVATE jni_zero)

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
elseif(IOS)
  # The Objective-C SDK, without the VP8/VP9/AV1 classes (this build has no
  # software video codecs; video uses VideoToolbox's H.264/H.265).
  target_sources(webrtc_sdk PRIVATE
    objc/api/logging/RTCCallbackLogger.mm
    objc/api/peerconnection/RTCAudioSource.mm
    objc/api/peerconnection/RTCAudioTrack.mm
    objc/api/peerconnection/RTCCertificate.mm
    objc/api/peerconnection/RTCConfiguration.mm
    objc/api/peerconnection/RTCCryptoOptions.mm
    objc/api/peerconnection/RTCDataChannel.mm
    objc/api/peerconnection/RTCDataChannelConfiguration.mm
    objc/api/peerconnection/RTCDtlsFingerprint.mm
    objc/api/peerconnection/RTCDtmfSender.mm
    objc/api/peerconnection/RTCEncodedImage+Private.mm
    objc/api/peerconnection/RTCFieldTrials.mm
    objc/api/peerconnection/RTCFileLogger.mm
    objc/api/peerconnection/RTCIceCandidate.mm
    objc/api/peerconnection/RTCIceCandidateErrorEvent.mm
    objc/api/peerconnection/RTCIceServer.mm
    objc/api/peerconnection/RTCLegacyStatsReport.mm
    objc/api/peerconnection/RTCMediaConstraints.mm
    objc/api/peerconnection/RTCMediaSource.mm
    objc/api/peerconnection/RTCMediaStream.mm
    objc/api/peerconnection/RTCMediaStreamTrack.mm
    objc/api/peerconnection/RTCMetrics.mm
    objc/api/peerconnection/RTCMetricsSampleInfo.mm
    objc/api/peerconnection/RTCPeerConnection.mm
    objc/api/peerconnection/RTCPeerConnection+DataChannel.mm
    objc/api/peerconnection/RTCPeerConnection+Stats.mm
    objc/api/peerconnection/RTCPeerConnectionFactory.mm
    objc/api/peerconnection/RTCPeerConnectionFactoryBuilder.mm
    objc/api/peerconnection/RTCPeerConnectionFactoryBuilder+DefaultComponents.mm
    objc/api/peerconnection/RTCPeerConnectionFactoryOptions.mm
    objc/api/peerconnection/RTCRtcpParameters.mm
    objc/api/peerconnection/RTCRtpCapabilities.mm
    objc/api/peerconnection/RTCRtpCodecCapability.mm
    objc/api/peerconnection/RTCRtpCodecParameters.mm
    objc/api/peerconnection/RTCRtpEncodingParameters.mm
    objc/api/peerconnection/RTCRtpHeaderExtension.mm
    objc/api/peerconnection/RTCRtpHeaderExtensionCapability.mm
    objc/api/peerconnection/RTCRtpParameters.mm
    objc/api/peerconnection/RTCRtpReceiver.mm
    objc/api/peerconnection/RTCRtpSender.mm
    objc/api/peerconnection/RTCRtpSource.mm
    objc/api/peerconnection/RTCRtpTransceiver.mm
    objc/api/peerconnection/RTCSessionDescription.mm
    objc/api/peerconnection/RTCSSLAdapter.mm
    objc/api/peerconnection/RTCStatisticsReport.mm
    objc/api/peerconnection/RTCTracing.mm
    objc/api/peerconnection/RTCVideoCodecInfo+Private.mm
    objc/api/peerconnection/RTCVideoEncoderSettings+Private.mm
    objc/api/peerconnection/RTCVideoSource.mm
    objc/api/peerconnection/RTCVideoTrack.mm
    objc/api/RTCVideoRendererAdapter.mm
    objc/api/video_codec/RTCNativeVideoDecoder.mm
    objc/api/video_codec/RTCNativeVideoEncoder.mm
    objc/api/video_codec/RTCVideoCodecConstants.mm
    objc/api/video_frame_buffer/RTCNativeI420Buffer.mm
    objc/api/video_frame_buffer/RTCNativeMutableI420Buffer.mm
    objc/base/RTCEncodedImage.m
    objc/base/RTCLogging.mm
    objc/base/RTCVideoCapturer.m
    objc/base/RTCVideoCodecInfo.m
    objc/base/RTCVideoEncoderFactory.mm
    objc/base/RTCVideoEncoderQpThresholds.m
    objc/base/RTCVideoEncoderSettings.m
    objc/base/RTCVideoFrame.mm
    objc/components/audio/RTCAudioSession.mm
    objc/components/audio/RTCAudioSession+Configuration.mm
    objc/components/audio/RTCAudioSessionConfiguration.m
    objc/components/audio/RTCNativeAudioSessionDelegateAdapter.mm
    objc/components/capturer/RTCCameraVideoCapturer.m
    objc/components/capturer/RTCFileVideoCapturer.m
    objc/components/network/RTCNetworkMonitor.mm
    objc/components/renderer/metal/RTCMTLI420Renderer.mm
    objc/components/renderer/metal/RTCMTLNV12Renderer.mm
    objc/components/renderer/metal/RTCMTLRenderer.mm
    objc/components/renderer/metal/RTCMTLRGBRenderer.mm
    objc/components/renderer/metal/RTCMTLVideoView.m
    objc/components/video_codec/helpers.cc
    objc/components/video_codec/nalu_rewriter.cc
    objc/components/video_codec/RTCCodecSpecificInfoH264.mm
    objc/components/video_codec/RTCDefaultVideoDecoderFactory.m
    objc/components/video_codec/RTCDefaultVideoEncoderFactory.mm
    objc/components/video_codec/RTCH264ProfileLevelId.mm
    objc/components/video_codec/RTCVideoDecoderFactoryH264.m
    objc/components/video_codec/RTCVideoDecoderH264.mm
    objc/components/video_codec/RTCVideoEncoderFactoryH264.m
    objc/components/video_codec/RTCVideoEncoderH264.mm
    objc/components/video_codec/UIDevice+H264Profile.mm
    objc/components/video_frame_buffer/RTCCVPixelBuffer.mm
    objc/helpers/AudioTimeStamp+Nanoseconds.mm
    objc/helpers/AVCaptureSession+DevicePosition.mm
    objc/helpers/NSString+StdString.mm
    objc/helpers/RTCCameraPreviewView.m
    objc/helpers/RTCDispatcher.m
    objc/helpers/UIDevice+RTCDevice.mm
    objc/native/api/audio_device_module.mm
    objc/native/api/network_monitor_factory.mm
    objc/native/api/objc_audio_device_module.mm
    objc/native/api/ssl_certificate_verifier.mm
    objc/native/api/video_capturer.mm
    objc/native/api/video_decoder_factory.mm
    objc/native/api/video_encoder_factory.mm
    objc/native/api/video_frame_buffer.mm
    objc/native/api/video_frame.mm
    objc/native/api/video_renderer.mm
    objc/native/src/audio/audio_device_ios.mm
    objc/native/src/audio/audio_device_module_ios.mm
    objc/native/src/audio/helpers.mm
    objc/native/src/audio/voice_processing_audio_unit.mm
    objc/native/src/objc_audio_device_delegate.mm
    objc/native/src/objc_audio_device.mm
    objc/native/src/objc_frame_buffer.mm
    objc/native/src/objc_network_monitor.mm
    objc/native/src/objc_video_decoder_factory.mm
    objc/native/src/objc_video_encoder_factory.mm
    objc/native/src/objc_video_frame.mm
    objc/native/src/objc_video_renderer.mm
    objc/native/src/objc_video_track_source.mm
  )
  target_include_directories(webrtc_sdk PRIVATE objc objc/base)
  target_compile_options(webrtc_sdk PRIVATE
    "$<$<COMPILE_LANGUAGE:OBJC,OBJCXX>:-fobjc-arc;-fno-objc-arc-exceptions>")

  # The framework's public headers (gn's common_objc_headers, without the
  # VP8/VP9/AV1 ones). Like gn, copy them flat into Headers/ with their
  # #import "sdk/objc/.../RTCFoo.h" lines rewritten to <WebRTC/RTCFoo.h>, and
  # generate the umbrella header WebRTC.h and the module map.
  set(webrtc_objc_headers
    objc/base/RTCCodecSpecificInfo.h
    objc/base/RTCEncodedImage.h
    objc/base/RTCI420Buffer.h
    objc/base/RTCLogging.h
    objc/base/RTCMacros.h
    objc/base/RTCMutableI420Buffer.h
    objc/base/RTCMutableYUVPlanarBuffer.h
    objc/base/RTCSSLCertificateVerifier.h
    objc/base/RTCVideoCapturer.h
    objc/base/RTCVideoCodecInfo.h
    objc/base/RTCVideoDecoder.h
    objc/base/RTCVideoDecoderFactory.h
    objc/base/RTCVideoEncoder.h
    objc/base/RTCVideoEncoderFactory.h
    objc/base/RTCVideoEncoderQpThresholds.h
    objc/base/RTCVideoEncoderSettings.h
    objc/base/RTCVideoFrame.h
    objc/base/RTCVideoFrameBuffer.h
    objc/base/RTCVideoRenderer.h
    objc/base/RTCYUVPlanarBuffer.h
    objc/components/audio/RTCAudioDevice.h
    objc/components/audio/RTCAudioSession.h
    objc/components/audio/RTCAudioSessionConfiguration.h
    objc/components/capturer/RTCCameraVideoCapturer.h
    objc/components/capturer/RTCFileVideoCapturer.h
    objc/components/network/RTCNetworkMonitor.h
    objc/components/renderer/metal/RTCMTLVideoView.h
    objc/components/renderer/opengl/RTCEAGLVideoView.h
    objc/components/renderer/opengl/RTCVideoViewShading.h
    objc/components/video_codec/RTCCodecSpecificInfoH264.h
    objc/components/video_codec/RTCDefaultVideoDecoderFactory.h
    objc/components/video_codec/RTCDefaultVideoEncoderFactory.h
    objc/components/video_codec/RTCH264ProfileLevelId.h
    objc/components/video_codec/RTCVideoDecoderFactoryH264.h
    objc/components/video_codec/RTCVideoDecoderH264.h
    objc/components/video_codec/RTCVideoEncoderFactoryH264.h
    objc/components/video_codec/RTCVideoEncoderH264.h
    objc/components/video_frame_buffer/RTCCVPixelBuffer.h
    objc/helpers/RTCCameraPreviewView.h
    objc/helpers/RTCDispatcher.h
    objc/helpers/UIDevice+RTCDevice.h
    objc/api/peerconnection/RTCAudioSource.h
    objc/api/peerconnection/RTCAudioTrack.h
    objc/api/peerconnection/RTCConfiguration.h
    objc/api/peerconnection/RTCDataChannel.h
    objc/api/peerconnection/RTCDataChannelConfiguration.h
    objc/api/peerconnection/RTCFieldTrials.h
    objc/api/peerconnection/RTCIceCandidate.h
    objc/api/peerconnection/RTCIceCandidateErrorEvent.h
    objc/api/peerconnection/RTCIceServer.h
    objc/api/peerconnection/RTCLegacyStatsReport.h
    objc/api/peerconnection/RTCMediaConstraints.h
    objc/api/peerconnection/RTCMediaSource.h
    objc/api/peerconnection/RTCMediaStream.h
    objc/api/peerconnection/RTCMediaStreamTrack.h
    objc/api/peerconnection/RTCMetrics.h
    objc/api/peerconnection/RTCMetricsSampleInfo.h
    objc/api/peerconnection/RTCPeerConnection.h
    objc/api/peerconnection/RTCPeerConnectionFactory.h
    objc/api/peerconnection/RTCPeerConnectionFactoryOptions.h
    objc/api/peerconnection/RTCRtcpParameters.h
    objc/api/peerconnection/RTCRtpCapabilities.h
    objc/api/peerconnection/RTCRtpCodecCapability.h
    objc/api/peerconnection/RTCRtpCodecParameters.h
    objc/api/peerconnection/RTCRtpEncodingParameters.h
    objc/api/peerconnection/RTCRtpHeaderExtension.h
    objc/api/peerconnection/RTCRtpHeaderExtensionCapability.h
    objc/api/peerconnection/RTCRtpParameters.h
    objc/api/peerconnection/RTCRtpReceiver.h
    objc/api/peerconnection/RTCRtpSource.h
    objc/api/peerconnection/RTCRtpSender.h
    objc/api/peerconnection/RTCRtpTransceiver.h
    objc/api/peerconnection/RTCDtmfSender.h
    objc/api/peerconnection/RTCSSLAdapter.h
    objc/api/peerconnection/RTCSessionDescription.h
    objc/api/peerconnection/RTCStatisticsReport.h
    objc/api/peerconnection/RTCTracing.h
    objc/api/peerconnection/RTCCertificate.h
    objc/api/peerconnection/RTCCryptoOptions.h
    objc/api/peerconnection/RTCDtlsFingerprint.h
    objc/api/peerconnection/RTCVideoSource.h
    objc/api/peerconnection/RTCVideoTrack.h
    objc/api/video_codec/RTCVideoCodecConstants.h
    objc/api/video_frame_buffer/RTCNativeI420Buffer.h
    objc/api/video_frame_buffer/RTCNativeMutableI420Buffer.h
    objc/api/logging/RTCCallbackLogger.h
    objc/api/peerconnection/RTCFileLogger.h
  )
  set(gen_dir ${CMAKE_CURRENT_BINARY_DIR}/framework)
  # Writes <file> only if its content changed, so headers that are rewritten
  # identically on a re-configure don't trigger rebuilds.
  function(webrtc_write_if_changed file content)
    file(WRITE ${file}.tmp "${content}")
    configure_file(${file}.tmp ${file} COPYONLY)
    file(REMOVE ${file}.tmp)
  endfunction()
  set(framework_headers ${gen_dir}/WebRTC.h)
  set(umbrella "")
  foreach(header ${webrtc_objc_headers})
    get_filename_component(name ${header} NAME)
    file(READ ${header} content)
    # gn's tools_webrtc/apple/copy_framework_header.py: #import "[path/]
    # [Category+]RTCFoo.h" -> #import <WebRTC/[Category+]RTCFoo.h>. (Four
    # passes, as CMake's regex can't refer to an optional group that didn't
    # match.)
    foreach(pattern
        "\"[^\"]*/RTC([A-Za-z0-9_]+)[.]h\";RTC\\1"
        "\"[^\"]*/([A-Za-z0-9_]+[+])RTC([A-Za-z0-9_]+)[.]h\";\\1RTC\\2"
        "\"RTC([A-Za-z0-9_]+)[.]h\";RTC\\1"
        "\"([A-Za-z0-9_]+[+])RTC([A-Za-z0-9_]+)[.]h\";\\1RTC\\2")
      list(GET pattern 0 match)
      list(GET pattern 1 replacement)
      string(REGEX REPLACE "#import[ \t]+${match}"
        "#import <WebRTC/${replacement}.h>" content "${content}")
    endforeach()
    webrtc_write_if_changed(${gen_dir}/${name} "${content}")
    list(APPEND framework_headers ${gen_dir}/${name})
    string(APPEND umbrella "#import <WebRTC/${name}>\n")
  endforeach()
  set_property(DIRECTORY APPEND PROPERTY CMAKE_CONFIGURE_DEPENDS
    ${webrtc_objc_headers})
  webrtc_write_if_changed(${gen_dir}/WebRTC.h "${umbrella}")
  webrtc_write_if_changed(${gen_dir}/module.modulemap
"framework module WebRTC {
  umbrella header \"WebRTC.h\"

  export *
  module * { export * }
}
")
  set_source_files_properties(${gen_dir}/module.modulemap PROPERTIES
    MACOSX_PACKAGE_LOCATION Modules)
  set_source_files_properties(objc/PrivacyInfo.xcprivacy PROPERTIES
    MACOSX_PACKAGE_LOCATION Resources)
  # CMake copies a framework's headers and resources before compiling the
  # framework target's own sources, so it needs one; all the code comes from
  # the OBJECT libraries above.
  webrtc_write_if_changed(${gen_dir}/framework.mm
    "// Empty: WebRTC.framework's code comes from WebRTC's OBJECT libraries.\n")

  # WebRTC.framework, linked like gn does: all of WebRTC's objects (gn uses
  # -all_load), with unused code dropped by -dead_strip.
  add_library(WebRTC SHARED
    ${gen_dir}/framework.mm
    ${framework_headers}
    ${gen_dir}/module.modulemap
    objc/PrivacyInfo.xcprivacy
  )
  set_target_properties(WebRTC PROPERTIES
    FRAMEWORK TRUE
    PUBLIC_HEADER "${framework_headers}"
    MACOSX_FRAMEWORK_IDENTIFIER org.webrtc.WebRTC
    MACOSX_FRAMEWORK_INFO_PLIST ${CMAKE_CURRENT_SOURCE_DIR}/objc/Info.plist.in
    MACOSX_FRAMEWORK_BUNDLE_VERSION 1.0
    MACOSX_FRAMEWORK_SHORT_VERSION_STRING 1.0
    LINKER_LANGUAGE OBJCXX
  )
  target_link_libraries(WebRTC PRIVATE
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
    webrtc_modules_video_capture
    webrtc_modules_video_coding
    webrtc_net
    webrtc_p2p
    webrtc_pc
    webrtc_rtc_base
    webrtc_sdk
    webrtc_system_wrappers
    webrtc_video
    absl boringssl libsrtp libyuv opus pffft rnnoise sframe
    "-framework AVFoundation" "-framework AudioToolbox" "-framework CoreAudio"
    "-framework CoreFoundation" "-framework CoreGraphics" "-framework CoreMedia"
    "-framework CoreVideo" "-framework Foundation" "-framework Metal"
    "-framework MetalKit" "-framework Network" "-framework QuartzCore"
    "-framework UIKit" "-framework VideoToolbox"
  )
  target_link_options(WebRTC PRIVATE -Wl,-dead_strip -Wl,-ObjC
    -Wl,-install_name,@rpath/WebRTC.framework/WebRTC)
endif()
