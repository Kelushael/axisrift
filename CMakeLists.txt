cmake_minimum_required(VERSION 3.22)

# Match the axistape target: Intel + High Sierra so it loads on old Macs too.
set(CMAKE_OSX_DEPLOYMENT_TARGET "10.13" CACHE STRING "" FORCE)
set(CMAKE_OSX_ARCHITECTURES "x86_64" CACHE STRING "" FORCE)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(AxisRift VERSION 1.0.0)

include(FetchContent)
FetchContent_Declare(JUCE
  GIT_REPOSITORY https://github.com/juce-framework/JUCE.git
  GIT_TAG 7.0.12
  GIT_SHALLOW TRUE)
FetchContent_MakeAvailable(JUCE)

juce_add_plugin(StubbsCue
  COMPANY_NAME "Axis Rift"
  BUNDLE_ID com.axisrift.stubbscue
  PLUGIN_MANUFACTURER_CODE Axrf
  PLUGIN_CODE Stbq
  FORMATS AU VST3 Standalone
  PRODUCT_NAME "Stubbs Cue"
  IS_SYNTH FALSE
  NEEDS_MIDI_INPUT FALSE
  COPY_PLUGIN_AFTER_BUILD FALSE)

target_sources(StubbsCue PRIVATE src/StubbsCue.cpp)
target_compile_definitions(StubbsCue PUBLIC
  JUCE_WEB_BROWSER=0 JUCE_USE_CURL=0 JUCE_VST3_CAN_REPLACE_VST2=0
  JUCE_DISPLAY_SPLASH_SCREEN=0 JUCE_REPORT_APP_USAGE=0)
target_link_libraries(StubbsCue PRIVATE
  juce::juce_audio_utils juce::juce_dsp
  PUBLIC juce::juce_recommended_config_flags juce::juce_recommended_warning_flags)
