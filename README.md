# CUEORGBPlugin
 Custom iCUE plugin to control OpenRGB from within iCUE
 
# Compiling
Should be no major dependencies, built with Visual Studio Community 2022. 

# Current bugs
* Colors and effects can only be individually set on 2 devices. Every device over 2 devices will follow the lighting sync effect(s). I assume that it is necessary to alter the CreadeDeviceInfo method to create devices with the other deivce types.
* It is currently not possible to control which device will be one of the 2 which can be individually controlled. I'm working on making a config file where the user can specify which 2 devices should be the individually controlable ones. This workaround will have to exist until I figure out how to set more than 2 device types that work with all functions.
* The plugin does not work on iCUE versions above 5.22 due to an issue with the plugin signature. Please stay on version 5.21 or lower for now.
 
# Description
This plugin allows creating custom device layouts using json files and custom images as well as generating some generic LED layouts if there is no defined device. Corsair has not released an official SDK for adding custom devices. The reverse engineering is contained in `CUESDKDevice.h`. 

![Custom Device](/screenshots/custom_device_v4.PNG)

**Files of Importance**
* `version.dll` - This is a wrapper dll to disable iCUE's signature check on plugins loaded from the Plugins folder. Normally iCUE will run WinVerifyTrust on all plugins it is attempting to load. I haven't determined whether it needs to be signed by Corsair specifically, this wrapper when placed in the Corsair iCUE program directory will disable this check entirely.
* `CUEORGBPlugin.dll` - This implements the OpenRGB Client interface and utilizes/merges json files to create devices
* `settings.json` - This file specifies the default LED/Zone layout display of some devices in the event no specific device is found within `devices.json`
* `devices.json` - This file maps devices by name given within OpenRGB and allows overriding the Default layout
 
# Release Installation
* Install at least 1 plugin in iCUE for the iCUE plugin host to be enabled.
* Close iCUE completely by right-clicking in the task bar and pressing `Quit`
* Copy contents of archive to `C:\Program Files\Corsair\CORSAIR iCUE 5 Software` directly.

# Manual Installation
* Install at least 1 plugin in iCUE for the iCUE plugin host to be enabled.
* Close iCUE completely by right-clicking in the task bar and pressing `Quit`
* Copy `plugins` folder to `C:\Program Files\Corsair\CORSAIR iCUE 5 Software`
* Copy built `version.dll` to `C:\Program Files\Corsair\CORSAIR iCUE 5 Software`
* Copy built `CUEORGBPlugin.dll` to `C:\Program Files\Corsair\CORSAIR iCUE 5 Software\plugins\OpenRGB`

# OpenRGB Installation
* Download [OpenRGB]((https://openrgb.org/releases.html))
* Place the folder anywhere (preferably in Program Files)
* Run OpenRGB
* Open the supported devices settings in OpenRGB and disable all Corsair devices to prevent conflicts (e.g. Commander Pro fan speeds may no longer show up after device scan).
* You might want to enable the "minimize on close" option in OpenRGB to prevent it from closing when the main window is closed.
* Navigate to `SDK Server` tab
* Press `Start Server`
* Run iCUE

OpenRGB should look like the following when iCUE connects:
![OpenRGB](/screenshots/open_rgb_server.PNG)

# OpenRGB as a Startup process (Windows)
You might want to start OpenRGB when you start windows as iCUE is also a Startup process, allowing OpenRGB to be started alongside will allow you to resume effects controlled by iCUE when both programs start
* Go to the settings in OpenRGB
* Go to the general settings, enable start on login, start minimized and start server.

# Thirdparty Projects used

* CUESDK - https://github.com/CorsairOfficial/cue-sdk
* OpenRGB - https://gitlab.com/CalcProgrammer1/OpenRGB
* Json C++ - https://github.com/nlohmann/json
* SHA256 - https://github.com/okdshin/PicoSHA2
