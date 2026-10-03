# The modules/video_capture/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_video_capture OBJECT
  device_info_impl.cc
  video_capture_factory.cc
  video_capture_impl.cc
  video_capture_options.cc
)
target_link_libraries(webrtc_modules_video_capture PRIVATE webrtc_config libyuv)
