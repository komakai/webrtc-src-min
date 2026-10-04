# The p2p/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_p2p ${WEBRTC_LIBRARY_TYPE}
  base/async_stun_tcp_socket.cc
  base/basic_async_resolver_factory.cc
  base/basic_ice_controller.cc
  base/basic_packet_socket_factory.cc
  base/connection.cc
  base/connection_info.cc
  base/default_ice_transport_factory.cc
  base/ice_controller_interface.cc
  base/ice_credentials_iterator.cc
  base/ice_switch_reason.cc
  base/ice_transport_internal.cc
  base/p2p_constants.cc
  base/p2p_transport_channel.cc
  base/packet_transport_internal.cc
  base/port.cc
  base/port_allocator.cc
  base/port_interface.cc
  base/stun_dictionary.cc
  base/stun_port.cc
  base/stun_request.cc
  base/tcp_port.cc
  base/transport_description.cc
  base/transport_description_factory.cc
  base/turn_port.cc
  base/wrapping_active_ice_controller.cc
  client/basic_port_allocator.cc
  client/turn_port_factory.cc
  dtls/dtls_stun_piggyback_controller.cc
  dtls/dtls_transport.cc
  dtls/dtls_transport_internal.cc
  dtls/dtls_utils.cc
)
target_link_libraries(webrtc_p2p PRIVATE webrtc_config)
