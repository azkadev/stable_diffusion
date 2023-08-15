cmake_minimum_required(VERSION 3.10)
project(stable_diffusion_flutter_library VERSION 0.0.0 LANGUAGES C CXX)

add_library(stable_diffusion_flutter SHARED
  "stable_diffusion.cpp"
)

set_target_properties(stable_diffusion_flutter PROPERTIES
  # PUBLIC_HEADER stable_diffusion_flutter.h
  OUTPUT_NAME "stable_diffusion"
)

target_compile_definitions(stable_diffusion_flutter PUBLIC DART_SHARED_LIB)
