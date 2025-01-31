cmake_minimum_required(VERSION 3.15)
set(CMAKE_OSX_DEPLOYMENT_TARGET "10.12" CACHE STRING "Minimum OS X deployment target")
set(CMAKE_CXX_STANDARD 17)
project(CHOWTapeModel VERSION 2.7.3)

add_subdirectory(modules)
include_directories(modules/RTNeural)

# set up plugin project
juce_set_vst2_sdk_path(/Users/shane/SDKs/VST_SDK/VST2_SDK/)
# include_directories(ASIO_SDK)

if(IOS)
    juce_add_plugin(CHOWTapeModel
        COMPANY_NAME chowdsp
        PLUGIN_MANUFACTURER_CODE Chow
        PLUGIN_CODE Jdox
        FORMATS Standalone AUv3
        ProductName "CHOWTapeModel"
        ICON_BIG Source/GUI/Assets/logo.png
        MICROPHONE_PERMISSION_ENABLED TRUE
    )
else()
    juce_add_plugin(CHOWTapeModel
        COMPANY_NAME chowdsp
        PLUGIN_MANUFACTURER_CODE Chow
        PLUGIN_CODE Jdox
        FORMATS AU VST3 Standalone LV2 VST
        ProductName "CHOWTapeModel"
        LV2_URI https://github.com/jatinchowdhury18/AnalogTapeModel
        ICON_BIG Source/GUI/Assets/logo.png
        MICROPHONE_PERMISSION_ENABLED TRUE
    )
endif()

# create JUCE header
juce_generate_juce_header(CHOWTapeModel)

add_subdirectory(Source)

target_compile_definitions(CHOWTapeModel
    PUBLIC
    JUCE_DISPLAY_SPLASH_SCREEN=0
    JUCE_REPORT_APP_USAGE=0
    JUCE_WEB_BROWSER=0
    JUCE_USE_CURL=0
    JUCE_VST3_CAN_REPLACE_VST2=0
)

target_link_libraries(CHOWTapeModel PUBLIC
    juce_plugin_modules
)

# we need these flags for notarization on MacOS
option(MACOS_RELEASE "Set build flags for MacOS Release" OFF)
if(MACOS_RELEASE)
    message(STATUS "Setting MacOS release flags...")
    set_target_properties(CHOWTapeModel_Standalone PROPERTIES
        XCODE_ATTRIBUTE_ENABLE_HARDENED_RUNTIME YES)
    # set_target_properties(ChowCentaur_AUv3 PROPERTIES
    #     XCODE_ATTRIBUTE_ENABLE_HARDENED_RUNTIME YES)
endif()

if(IOS)
    message(STATUS "Setting iOS-specific properties...")

    foreach(target IN ITEMS BinaryData juce_plugin_modules CHOWTapeModel CHOWTapeModel_Standalone CHOWTapeModel_AUv3)
        set_target_properties(${target}
            PROPERTIES
                RUNTIME_OUTPUT_DIRECTORY "./"
                ARCHIVE_OUTPUT_DIRECTORY "./"
                LIBRARY_OUTPUT_DIRECTORY "./")
    endforeach()

    set_target_properties(CHOWTapeModel_Standalone PROPERTIES
            XCODE_ATTRIBUTE_INSTALL_PATH "$(LOCAL_APPS_DIR)"
            XCODE_ATTRIBUTE_SKIP_INSTALL "NO")

    set_target_properties(CHOWTapeModel_AUv3 PROPERTIES
            XCODE_ATTRIBUTE_INSTALL_PATH "$(LOCAL_APPS_DIR)/CHOWTapeModel.app/PlugIns"
            XCODE_ATTRIBUTE_SKIP_INSTALL "NO")
endif()
