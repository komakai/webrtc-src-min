# WebRTC's Android library, libjingle_peerconnection_so.so, or its iOS
# framework, WebRTC.framework, built from the sources of the gn targets they
# depend on (one OBJECT library per directory, and per modules/ subdirectory).
# Needs the targets from webrtc-third_party-min (absl, boringssl, libyuv, ...)
# and the compile flags from webrtc-min's top-level CMakeLists.txt.
cmake_minimum_required(VERSION 3.22)
project(webrtc C CXX ASM)
if(APPLE)
  enable_language(OBJC OBJCXX)
endif()

if(CMAKE_SYSTEM_PROCESSOR MATCHES "^(aarch64|arm64)")
  set(WEBRTC_CPU arm64)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^arm")
  set(WEBRTC_CPU arm)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^(x86_64|AMD64)$")
  set(WEBRTC_CPU x64)
elseif(CMAKE_SYSTEM_PROCESSOR MATCHES "^(i.86|x86)$")
  set(WEBRTC_CPU x86)
else()
  message(FATAL_ERROR "webrtc: unsupported CPU ${CMAKE_SYSTEM_PROCESSOR}")
endif()

# The defines gn's WebRTC configs set for this build: rtc_* gn args as built
# by webrtc-min (no software video codecs, no protobuf, no Rust), plus the few
# that gn sets only on some targets.
add_library(webrtc_config INTERFACE)
target_include_directories(webrtc_config INTERFACE ${CMAKE_CURRENT_SOURCE_DIR})
target_compile_definitions(webrtc_config INTERFACE
  PROTOBUF_ENABLE_DEBUG_LOGGING_MAY_LEAK_PII=0
  RTC_DAV1D_IN_INTERNAL_DECODER_FACTORY
  RTC_ENABLE_VP9
  WEBRTC_APM_DEBUG_DUMP=0
  WEBRTC_CODEC_OPUS
  WEBRTC_DEPRECATE_PLAN_B
  WEBRTC_ENABLE_AVX2
  WEBRTC_ENABLE_PROTOBUF=0
  WEBRTC_ENCODER_PSNR_STATS
  WEBRTC_HAVE_DCSCTP
  WEBRTC_HAVE_SCTP
  WEBRTC_INCLUDE_INTERNAL_AUDIO_DEVICE
  WEBRTC_LIBRARY_IMPL
  WEBRTC_NON_STATIC_TRACE_EVENT_HANDLERS=0
  WEBRTC_NO_SOFTWARE_VIDEO_CODECS
  WEBRTC_OPUS_SUPPORT_120MS_PTIME=1
  WEBRTC_OPUS_VARIABLE_COMPLEXITY=0
  WEBRTC_POSIX
  WEBRTC_STRICT_FIELD_TRIALS=0
  WEBRTC_USE_BUILTIN_OPUS=1
  WEBRTC_WITHOUT_RUST
)
if(ANDROID)
  target_compile_definitions(webrtc_config INTERFACE WEBRTC_ANDROID WEBRTC_LINUX)
elseif(IOS)
  target_compile_definitions(webrtc_config INTERFACE
    WEBRTC_IOS WEBRTC_MAC WEBRTC_ENABLE_OBJC_SYMBOL_EXPORT)
else()
  message(FATAL_ERROR "webrtc: only Android and iOS are supported")
endif()
if(WEBRTC_CPU STREQUAL "arm64")
  target_compile_definitions(webrtc_config INTERFACE WEBRTC_ARCH_ARM64 WEBRTC_HAS_NEON)
elseif(WEBRTC_CPU STREQUAL "arm")
  target_compile_definitions(webrtc_config INTERFACE
    WEBRTC_ARCH_ARM WEBRTC_ARCH_ARM_V7 WEBRTC_HAS_NEON)
endif()
target_link_libraries(webrtc_config INTERFACE absl chromium_src_root)

add_subdirectory(api)
add_subdirectory(audio)
add_subdirectory(call)
add_subdirectory(common_audio)
add_subdirectory(common_video)
add_subdirectory(logging)
add_subdirectory(media)
add_subdirectory(modules/async_audio_processing)
add_subdirectory(modules/audio_coding)
add_subdirectory(modules/audio_device)
add_subdirectory(modules/audio_mixer)
add_subdirectory(modules/audio_processing)
add_subdirectory(modules/congestion_controller)
add_subdirectory(modules/pacing)
add_subdirectory(modules/remote_bitrate_estimator)
add_subdirectory(modules/rtp_rtcp)
add_subdirectory(modules/sframe)
add_subdirectory(modules/third_party)
if(ANDROID)
  add_subdirectory(modules/utility)  # JNI helpers only
endif()
add_subdirectory(modules/video_capture)
add_subdirectory(modules/video_coding)
add_subdirectory(net)
add_subdirectory(p2p)
add_subdirectory(pc)
add_subdirectory(rtc_base)
add_subdirectory(sdk)
add_subdirectory(system_wrappers)
add_subdirectory(video)
