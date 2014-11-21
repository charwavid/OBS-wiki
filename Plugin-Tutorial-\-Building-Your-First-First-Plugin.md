You should now have your build environment set up. If you don't, take a look at the [[previous tutorial|Plugin-Tutorial-\-Setting-Up-Your-Environment]] to figure out how.

The plugin interface is actually quite simple. The bare minimum that you really need to do in order to make a plugin is to define a few functions and export them. These functions are `LoadPlugin`, `UnloadPlugin`, `GetPluginName`, and `GetPluginDescription`. Create a header file (`TutorialPlugin.h`) and prototype them with the following:

    #pragma once
    #include "OBSApi.h"
    extern "C" __declspec(dllexport) bool LoadPlugin();
    extern "C" __declspec(dllexport) void UnloadPlugin();
    extern "C" __declspec(dllexport) CTSTR GetPluginName();
    extern "C" __declspec(dllexport) CTSTR GetPluginDescription();

You can then define them with some stub code in `TutorialPlugin.cpp`

    #include "TutorialPlugin.h"
    
    bool LoadPlugin()
    {
        return true;
    }
    
    void UnloadPlugin()
    {
    }
    
    CTSTR GetPluginName()
    {
        return TEXT("Tutorial plugin");
    }
    
    CTSTR GetPluginDescription()
    {
        return TEXT("This plugin is designed to help teach people how to write plugins for OBS.");
    }

If that compiles fine, you should now have a plugin DLL in `rundir/plugins/`. If you start OBS and look at your plugins, you should see your brand new tutorial plugin! Now let's do something more interesting with this plugin.

[[Previous Tutorial: Setting Up Your Environment|Plugin-Tutorial-\-Setting-Up-Your-Environment]]

[[Next Tutorial: Create A New Video Source|Plugin-Tutorial-\-Create-A-New-Video-Source]]