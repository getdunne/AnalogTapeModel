target_sources(CHOWTapeModel PRIVATE
    Chew/ChewProcessor.cpp
    Degrade/DegradeProcessor.cpp

    Hysteresis/HysteresisProcessing.cpp
    Hysteresis/HysteresisProcessor.cpp
    Hysteresis/HysteresisSTN.cpp
    Hysteresis/ToneControl.cpp

    Input_Filters/InputFilters.cpp
    Loss_Effects/AzimuthProc.cpp
    Loss_Effects/LossFilter.cpp
    Timing_Effects/WowFlutterProcessor.cpp
    Timing_Effects/FlutterProcess.cpp
    Timing_Effects/WowProcess.cpp
)
