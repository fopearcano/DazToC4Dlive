# Daz To Cinema 4D Bridge
A Daz Studio Plugin based on Daz Bridge Library, allowing transfer of Daz Studio characters and
props to Cinema 4D.

* Owner: [Daz 3D][OwnerURL] – [@Daz3d][TwitterURL]
* License: [Apache License, Version 2.0][LicenseURL] - see ``LICENSE`` and ``NOTICE`` for more information.
* Offical Release: [Daz to Cinema 4D Bridge][ProductURL]
* Official Project: [github.com/daz3d/DazToC4D][RepositoryURL]


## Table of Contents
1. About the Bridge
2. Prerequisites
3. How to Install
4. How to Use
5. How to Build
6. How to QA Test
7. How to Develop
8. Directory Structure

## 1. About the Bridge
This is a refactored version of the original DazToC4D Bridge using the Daz Bridge Library as a foundation. Using the Bridge Library allows it to share source code and features with other bridges such as the refactored DazToUnity and DazToBlender bridges. This will improve development time and quality of all bridges.

The Daz To Cinema 4D Bridge consists of two parts: a Daz Studio plugin which exports assets to Cinema 4D and a Cinema 4D plugin which contains scripts and other resources to help recreate the look of the original Daz Studio asset in Cinema 4D.


## 2. Prerequisites
- A compatible version of the [Daz Studio][DazStudioURL] application
  - Minimum: 4.10
- A compatible version of the [Cinema 4D][Cinema4DURL] application
  - Minimum: R21 (Older versions may work with limited capabilities)
- Operating System:
  - Windows 7 or newer
  - macOS 10.13 (High Sierra) or newer

Daz Studio 4.22+ and Cinema 4D 2023+ should be used to take full advantage of the latest features of this plugin.

## 3. How to Install
### Daz Studio ###
- You can install the Daz To Cinema 4D Bridge automatically through the Daz Install Manager.  This will automatically add a new menu option under File -> Send To -> Daz To Cinema 4D.
- Alternatively, you can manually install by downloading the latest build from Github Release Page and following the instructions there to install into Daz Studio.

### Cinema 4D ###
1. Cinema 4D no longer requires a Plugins subfolder in the location where Cinema 4D was installed.  Since R20, users should set a plugins path inside the Cinema 4D Preferences window and install plugins to that folder.  We recommend creating a "**`\Documents\Cinema4D\Plugins`**" folder in your user's home folder and selecting that as the Plugins path in Cinema 4D.  Please refer to this link for more information: [Where do I install plugins? - Cinema 4D Knowledge Base](https://support.maxon.net/hc/en-us/articles/1500006433061-Where-do-I-install-plugins-)
2. The Daz Studio Plugin comes embedded with an installer for the Cinema 4D plugin.  From the Daz To Cinema 4D Bridge Dialog, there is now section in the Advanced Settings section for Installing the Cinema 4D Plugin.
3. Click the "Install Plugin" button.  You will see a window popup to choose a folder to install the Cinema 4D plugin.  
4. Navigate to the plugins folder path which you set from the Cinema 4D Preferences window, and click "Select Folder".  You will then see a confirmation dialog stating if the plugin installation was successful.
5. If Cinema 4D is running, you will need to restart for the Daz To Cinema 4D Bridge plugin to load.
6. In Cinema 4D, you should now see "Daz 3D" in the Cinema 4D main menu.


## 4. How to Use
1. Open your character in Daz Studio.
2. Make sure any clothing or hair is parented to the main body.
3. From the main menu, select File -> Send To -> Daz To Cinema 4D.  Alternatively, you may select File -> Export and then choose "Cinema 4D" from the Save as type drop down option.
4. A dialog will pop up: choose what type of conversion you wish to do, "Static Mesh" (no skeleton), "Skeletal Mesh" (Character or with joints), "Animation", or "Environment" (all meshes in scene).
5. To enable Morphs or Subdivision levels, click the CheckBox to Enable that option, then click the "Choose Morphs" or "Bake  Subdivisions" button to configure your selections.
6. Click Accept, then wait for a dialog popup to notify you when to switch to Cinema 4D.
7. From Cinema 4D, select Daz 3D -> Daz to C4D from the main menu. A DazToC4D dialog window should appear.
8. For Daz Characters or other assets transferred with the "Skeletal Mesh" option, select `GENESIS CHARACTERS`.  For props or other assets transferred using the "Static Mesh" or "Environment" option, select `ENVIRONMENTS + PROPS`.

### Morphs ###
- If you enabled the Export Morphs option, there will be a new "Morph Controller Group" node in the Object Manager panel.  Select this node and you will see morph sliders appear in the "Attributes Manager" panel, under the "User Data" heading.

### Animation ###
- To use the "Animation" asset type option, your Figure must use animations on the Daz Studio "Timeline" system.  
- If you are using "aniMate" or "aniBlocks" based animations, you need to right-click in the "aniMate" panel and select "Bake To Studio Keyframes".  
- Once your animation is on the "Timeline" system, you can start the transfer using File -> Send To -> Daz To Cinema 4D.  
- In Cinema 4D, click the "GENESIS CHARACTERS" from the DazToC4D window.  Your character with animations should begin to import.  During the import procedure, DazToC4D will notify you that "Importing Posed Figure is not fully supported" and ask if you want to "fix bone orientation".  Click "No".  
- If you accidentally click "Yes", the animation frames will not import correctly.  To fix that, you can just restart the import by clicking the "GENESIS CHARACTERS" button from the DazToC4D window.
- The transferred animation should now be usable through the Cinema 4D Animation interface.

### Subdivision Support ###
- Daz Studio uses Catmull-Clark Subdivision Surface technology which is a mathematical way to describe an infinitely smooth surface in a very efficient manner. Similar to how an infinitely smooth circle can be described with just the radius, the base resolution mesh of a Daz Figure is actually the mathematical data in an equation to describe an infinitely smooth surface. For Software which supports Catmull-Clark Subdivision and subdivision surface-based morphs (also known as HD Morphs), there is no loss in quality or detail by exporting the base resolution mesh (subdivision level 0).
- For Software which does not fully support Catmull-Clark Subdivision or HD Morphs, we can "Bake" additional subdivision detail levels into the mesh to more closely approximate the detail of the original surface. However, baking each additional subdivision level requires exponentially more CPU time, memory, and storage space.  **If you do not have a high-end PC, it is likely that your system will run out of memory and crash if you set the exported subdivision level above 2.**
- When you enable Bake Subdivision options in the Daz To Cinema 4D bridge, the asset is transferred to Cinema 4D as a standard mesh with higher resolution vertex counts.


## 5. How to Build
Setup and configuration of the build system is done via CMake to generate project files for Windows or Mac.  The CMake configuration requires:
-	Modern CMake (tested with 3.27.2 on Win and 3.27.0-rc4 on Mac)
-	Daz Studio 4.5+ SDK (from DIM)
-	Fbx SDK 2020.1 (win) / Fbx SDK 2015.1 (mac)
-	OpenSubdiv 3.4.4

(Please note that you MUST use the Qt 4.8.1 build libraries that are built-into the Daz Studio SDK.  Using an external Qt library will result in build errors and program instability.)

Download or clone the DazToC4D github repository to your local machine. The Daz Bridge Library is linked as a git submodule to the DazBridge repository. Depending on your git client, you may have to use `git submodule init` and `git submodule update` to properly clone the Daz Bridge Library.

The build setup process is designed to be run with CMake gui in an interactive session.  After setting up the source code folder and an output folder, the user can click Configure.  CMake will stop during the configurtaion process to prompt the user for the following paths:

-	DAZ_SDK_DIR – the root folder to the Daz Studio 4.5+ SDK.  This MUST be the version purchased from the Daz Store and installed via the DIM.  Any other versions will NOT work with this source code project and result in build errors and failure. example: C:/Users/Public/Documents/My DAZ 3D Library/DAZStudio4.5+ SDK
-	DAZ_STUDIO_EXE_DIR – the folder containing the Daz Studio executable file.  example: C:/Program Files/DAZ 3D/DAZStudio4
-	FBX_SDK_DIR – the root folder containing the “include” and “lib” subfolders.  example: C:/Program Files/Autodesk/FBX/FBX SDK/2020.0.1
-	OPENSUBDIV_DIR – root folder containing the “opensubdiv”, “examples”, “cmake” folders.  It assumes the output folder was set to a subfolder named “build” and that the osdCPU.lib or libosdCPU.a static library files were built at: <root>/build/lib/Release/osdCPU.lib or <root>/build/lib/Release/libosdCPU.a.  A pre-built library for Mac and Windows can be found at https://github.com/danielbui78/OpenSubdiv/releases that contains the correct location for include and prebuilt Release static library  binaries.  If you are not using this precompiled version, then you must ensure the correct location for the OPENSUBDIV_INCLUDE folder path and OPENSUBDIV_LIB filepath.

Once these paths are correctly entered into the CMake gui, the Configure button can be clicked and the configuration process should resume to completion.  The project files can then be generated and the project may be opened.  Please note that a custom version of Qt 4.8 build tools and libraries are included in the DAZ_SDK_DIR.  If another version of Qt is installed in your system and visible to CMake, it will likely cause errors with finding the correct version of Qt supplied in the DAZ_SDK_DIR and cause build errors and failure.

The resulting project files should have “DzBridge-C4D", “DzBridge Static” and "C4D Plugin ZIP" as project targets.  The DLL/DYLIB binary file produced by "DzBridge-C4D" should be a working Daz Studio plugin.  The "C4D Plugin ZIP" project contains the automation scripts which package the Cinema 4D Plugin files into a zip file and prepares it for embedding into the main Daz Studio plugin DLL/DYLIB binary.


