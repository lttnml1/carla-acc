# Source
Please note - most of this code was taken from Eleni Zapridou's github page https://github.com/ezapridou/carla-acc which was used in research published in "Runtime Verification of Autonomous Driving Systems in CARLA" [RV2020].  I am working to reproduce some of her results and extend her work in runtime verification for future research.  I have left comments in the code that point to original authorship by Zapridou where appropriate.

# System/Versions
CARLA: 0.9.6
UnrealEngine: 4.22
OS: Ubuntu 18.04
Python: 2.7 (due to the requirement currently to run the 0.0.9 version of rtamt)

# Adaptive Cruise Control System (ACC) in the [CARLA Simulator](https://carla.readthedocs.io/en/latest/)
This repository contains the files that were extended in the CARLA Simulator in order to implement an ACC system. This system was used to experiment with the runtime verification approach proposed in "Runtime Verification of Autonomous Driving Systems in CARLA" (published in [RV2020](https://rv20.ait.ac.at/) by E. Zapridou, E. Bartocci, P. Katsaros).

## ACC extension
In the folder [ACC](https://github.com/lttnml1/carla-acc/tree/main/ACC) you can find the files that were extended to implement the ACC system. The paths of these files are the following:
```bash
Unreal/CarlaUE4/Plugins/Carla/Source/Carla/Vehicle/WheeledVehicleAIController.h
Unreal/CarlaUE4/Plugins/Carla/Source/Carla/Vehicle/WheeledVehicleAIController.cpp
```

Additionally, the functionality of enabling/disabling the ACC system using the Python API was added. You can find the files that were extended to add this functionality in the folder [enable-disable ACC](https://github.com/lttnml1/carla-acc/tree/main/enable-disable%20ACC) as well as the root folder [carla-acc](https://github.com/lttnml1/carla-acc). Their paths are the following:
```bash
LibCarla/source/carla/client/detail/Client.h
LibCarla/source/carla/client/detail/Client.cpp
LibCarla/source/carla/client/detail/Simulator.h
LibCarla/source/carla/client/Vehicle.cpp
LibCarla/source/carla/client/Vehicle.h
LibCarla/source/carla/rpc/Command.h
PythonAPI/carla/source/libcarla/Actor.cpp
PythonAPI/carla/source/libcarla/Commands.cpp
Unreal/CarlaUE4/Plugins/Carla/Source/Carla/Server/CarlaServer.cpp
```
Please use the CarlaServer.cpp located in the root folder and not the [enable-disable ACC] folder.

I also found one more error in the Carla source code for the ObstacleDetectionSensor (always returned "50" for distance) so do this one as well:
Unreal/CarlaUE4/Plugins/Carla/Source/Carla/Sensor/ObstacleDetectionSensor.cpp

If you have already built CARLA and wish to add this ACC extension, please replace the original CARLA files with the ones found in these two folders and build again the simulator and the Python API. 
```bash
make PythonAPI
make launch
```

## Python Script
The script "acc_runtime_monitoring.py" is used to implement the rtamt library in order to monitor the ACC system in runtime. The requirement to be checked by the monitor is set as a constant inside the script (i.e., R, R1 or R2).  Currently this uses rtamt 0.0.9 (pip install real-time-analog-monitoring-tool==0.0.9) which is only compatible with Python 2, hence the reason for using Ubuntu 18.04 which still includes Python 2.7.17 by default.
