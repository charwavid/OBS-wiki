This series of tutorials assumes that you have a recent copy of Visual Studio installed. If you don't currently have Visual Studio, you can grab the Express edition [from Microsoft](http://www.visualstudio.com/). This tutorial is written with Visual Studio 2013 Express in mind, however any modern version should do.

To get started developing plugins, you'll also need the source code for Open Broadcaster Software. The best way to obtain the code is to grab it from the [official Github repository](https://github.com/jp9000/OBS).

Once you have the source code, you'll need to make sure that you have the latest [Windows SDK](http://www.microsoft.com/en-us/download/details.aspx?id=8279) and [DirectX SDK](http://www.microsoft.com/en-us/download/details.aspx?id=6812). 

For those of you who merely skim these tutorials, the things you need are as follows. If any of the links are dead, just do a quick web search for them. They aren't hard to find.
* [A recent version of Visual Studio](http://www.visualstudio.com/)
* [The OBS source](https://github.com/jp9000/OBS)
* [The latest Windows SDK](http://www.microsoft.com/en-us/download/details.aspx?id=8279)
* [The latest DirectX SDK](http://www.microsoft.com/en-us/download/details.aspx?id=6812)

Once you have all of that downloaded and installed, you want to set the following environment variables:
* `WindowsSDK80Path` - Set this to the path of your Windows SDK version. It doesn't actually matter which specific version of the SDK you have provided that it's Vista or newer, however newer is always better. Make sure you include the trailing slash.
* `DXSDK_DIR` - Set this to the path to your DirectX SDK. This should be set automatically by the DirectX installer, but if you run into issues, make sure that it actually got set. Make sure you include the trailing slash.
* `OBSSourcePath` (optional) - Set this to the path of your OBS source directory. You only need to set this if you want to use your own solution instead of adding your project to the `OBS-All` solution. You can also get away with not setting this variable if you manually replace references to it in this tutorial with the path. Make sure you include the trailing slash.

Now that you've done all of that, you're ready to compile OBS. Open the `OBS-All` solution in Visual Studio and build it. After a few minutes, you should have a fresh new copy of OBS inside of the `rundir` folder inside of the OBS source folder. To get OBS to run, you'll need to copy the `libx264` dll from the `x264` folder to `rundir`.

Now that you've successfully compiled OBS, you're ready to start creating plugins for it. It's easiest to add your plugin project to the OBS solution, but you can manage to develop a plugin with or without doing so. Right-click the project for your plugin and go to properties.

First, make sure you set up your project to compile as a `dll`. Under `Configuration Properties`, click `General` and change the following values:
* `Target Extension` - `.dll`
* `Configuration Type` - `Dynamic Library (.dll)`
* `Character Set` - `Use Unicode Character Set`
![What the screen should look like](http://i.frogbox.es/44l.png)

Under VC++ Directories, change the following values:
* `Include Directories` - `$(WindowsSDK80Path)Include\um;$(WindowsSDK80Path)Include\shared;$(DXSDK_DIR)Include;$(OBSSourcePath);$(IncludePath)`
* `Library Directories` - `$(WindowsSDK80Path)Lib\win8\um\x86;$(DXSDK_DIR)Lib\x86;$(OBSSourcePath);$(LibraryPath)`
![](http://i.frogbox.es/44m.png)

\*If you added your project to the `OBS-All` solution, you can omit `$(OBSSourcePath);` in both cases 

Under `C/C++`, click `General` and set the following:
* `Additional Include Directories` - `$(OBSSourcePath)OBSApi;%(AdditionalIncludeDirectories)`
![](http://i.frogbox.es/44o.png)

Under `Linker`, click `General` and set the following:
* `Output File` - `$(OBSSourcePath)rundir/plugins/$(TargetName)$(TargetExt)`
* `Additional Library Directories` - `$(OBSSourcePath)OBSApi/Debug;%(AdditionalLibraryDirectories)`
![](http://i.frogbox.es/44r.png)

Under `Linker`, click `Input` and set the following:
* `Additional Dependencies` - `OBSApi.lib;strmiids.lib;%(AdditionalDependencies)`
![](http://i.frogbox.es/44t.png)

Now that you've gotten all of that set up, you're ready to actually start programming!

[[Next Tutorial: Building Your First Plugin|Plugin-Tutorial-\-Building-Your-First-First-Plugin]]