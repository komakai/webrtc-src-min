# The modules/sframe/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_sframe ${WEBRTC_LIBRARY_TYPE}
  sframe_context_factory.cc
  sframe_encryptor.cc
)
target_link_libraries(webrtc_modules_sframe PRIVATE webrtc_config sframe)
