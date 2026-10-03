# The modules/third_party/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_third_party OBJECT
  fft/fft.c
  g711/g711.c
  g722/g722_decode.c
  g722/g722_encode.c
)
target_link_libraries(webrtc_modules_third_party PRIVATE webrtc_config)
