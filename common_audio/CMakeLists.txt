# The common_audio/ sources of the gn targets libjingle_peerconnection_so
# depends on.
add_library(webrtc_common_audio OBJECT
  audio_converter.cc
  audio_util.cc
  channel_buffer.cc
  real_fourier.cc
  real_fourier_ooura.cc
  resampler/push_resampler.cc
  resampler/push_sinc_resampler.cc
  resampler/resampler.cc
  resampler/sinc_resampler.cc
  ring_buffer.c
  signal_processing/auto_corr_to_refl_coef.c
  signal_processing/auto_correlation.c
  signal_processing/copy_set_operations.c
  signal_processing/cross_correlation.c
  signal_processing/division_operations.c
  signal_processing/dot_product_with_scale.cc
  signal_processing/downsample_fast.c
  signal_processing/energy.c
  signal_processing/filter_ar.c
  signal_processing/filter_ma_fast_q12.c
  signal_processing/get_hanning_window.c
  signal_processing/get_scaling_square.c
  signal_processing/levinson_durbin.c
  signal_processing/lpc_to_refl_coef.c
  signal_processing/min_max_operations.c
  signal_processing/randomization_functions.c
  signal_processing/refl_coef_to_lpc.c
  signal_processing/resample.c
  signal_processing/resample_48khz.c
  signal_processing/resample_by_2.c
  signal_processing/resample_by_2_internal.c
  signal_processing/resample_fractional.c
  signal_processing/spl_init.c
  signal_processing/spl_inl.c
  signal_processing/spl_sqrt.c
  signal_processing/splitting_filter.c
  signal_processing/vector_operations.c
  signal_processing/vector_scaling_operations.c
  smoothing_filter.cc
  third_party/ooura/fft_size_128/ooura_fft.cc
  third_party/ooura/fft_size_256/fft4g.cc
  vad/vad.cc
  vad/vad_core.c
  vad/vad_filterbank.c
  vad/vad_gmm.c
  vad/vad_sp.c
  vad/webrtc_vad.c
  wav_file.cc
  wav_header.cc
  window_generator.cc
)
if(WEBRTC_CPU STREQUAL "arm64" OR WEBRTC_CPU STREQUAL "arm")
  target_sources(webrtc_common_audio PRIVATE
    fir_filter_neon.cc
    resampler/sinc_resampler_neon.cc
    signal_processing/cross_correlation_neon.c
    signal_processing/downsample_fast_neon.c
    signal_processing/min_max_operations_neon.c
    third_party/ooura/fft_size_128/ooura_fft_neon.cc
  )
endif()
if(WEBRTC_CPU STREQUAL "arm64" OR WEBRTC_CPU STREQUAL "x64" OR WEBRTC_CPU STREQUAL "x86")
  target_sources(webrtc_common_audio PRIVATE
    signal_processing/filter_ar_fast_q12.c
    third_party/spl_sqrt_floor/spl_sqrt_floor.c
  )
endif()
if(WEBRTC_CPU STREQUAL "arm")
  target_sources(webrtc_common_audio PRIVATE
    signal_processing/filter_ar_fast_q12_armv7.S
    third_party/spl_sqrt_floor/spl_sqrt_floor_arm.S
  )
endif()
if(WEBRTC_CPU STREQUAL "x64" OR WEBRTC_CPU STREQUAL "x86")
  target_sources(webrtc_common_audio PRIVATE
    fir_filter_avx2.cc
    fir_filter_sse.cc
    resampler/sinc_resampler_avx2.cc
    resampler/sinc_resampler_sse.cc
    third_party/ooura/fft_size_128/ooura_fft_sse2.cc
  )
endif()
if(WEBRTC_CPU STREQUAL "x64" OR WEBRTC_CPU STREQUAL "x86")
  set_source_files_properties(
    fir_filter_avx2.cc
    resampler/sinc_resampler_avx2.cc
    PROPERTIES COMPILE_OPTIONS "-mavx2;-mfma")
endif()
if(WEBRTC_CPU STREQUAL "x64" OR WEBRTC_CPU STREQUAL "x86")
  set_source_files_properties(
    fir_filter_sse.cc
    resampler/sinc_resampler_sse.cc
    third_party/ooura/fft_size_128/ooura_fft.cc
    third_party/ooura/fft_size_128/ooura_fft_sse2.cc
    PROPERTIES COMPILE_OPTIONS "-msse2")
endif()
target_link_libraries(webrtc_common_audio PRIVATE webrtc_config)
