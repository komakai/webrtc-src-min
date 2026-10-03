# The modules/pacing/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_modules_pacing OBJECT
  bitrate_prober.cc
  interval_budget.cc
  pacing_controller.cc
  packet_router.cc
  prioritized_packet_queue.cc
  task_queue_paced_sender.cc
)
target_link_libraries(webrtc_modules_pacing PRIVATE webrtc_config)
