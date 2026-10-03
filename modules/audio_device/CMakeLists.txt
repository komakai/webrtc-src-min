# The modules/audio_device/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_audio_device OBJECT
  audio_device_buffer.cc
  audio_device_data_observer.cc
  audio_device_generic.cc
  audio_device_impl.cc
  dummy/audio_device_dummy.cc
  dummy/file_audio_device.cc
  dummy/file_audio_device_factory.cc
  fine_audio_buffer.cc
)
target_link_libraries(webrtc_modules_audio_device PRIVATE webrtc_config)
