# Dolphin - A GameCube / Wii / Triforce Emulator

[Homepage](https://dolphin-emu.org/) | [Project Site](https://github.com/dolphin-emu/dolphin) | [Forums](https://forums.dolphin-emu.org/) | [Wiki](https://wiki.dolphin-emu.org/) | [Issue Tracker](https://code.google.com/p/dolphin-emu/issues/list) | [Coding Style](https://github.com/dolphin-emu/dolphin/blob/master/Contributing.md) | [Transifex Page](https://www.transifex.com/projects/p/dolphin-emu/)

Dolphin is an emulator for running GameCube, Wii, and Triforce games on
Windows, Linux, OS X, and recent Android devices. It's licensed under
the terms of the GNU General Public License, version 2 (GPLv2).

Please read the [FAQ](https://dolphin-emu.org/docs/faq/) before using Dolphin.

## System Requirements
* OS
    * Microsoft Windows (Vista or higher).
    * Linux.
    * Apple Mac OS X (10.9 or higher).
    * Unix-like systems other than Linux might work but are not officially supported.
* Processor
    * A CPU with SSE2 support.
    * A modern CPU (3 GHz and Dual Core, not older than 2008) is highly recommended.
* Graphics
    * A reasonably modern graphics card (Direct3D 10.0 / OpenGL 3.0).
    * A graphics card that supports Direct3D 11 / OpenGL 4.4 is recommended.

## Installation on Windows
Use the solution file `Source/dolphin-emu.sln` to build Dolphin on Windows.
Visual Studio 2013 Update 3 is a hard requirement since previous versions don't support
many C++ features that we use. Other compilers might be able to build Dolphin
on Windows but have not been tested and are not recommended to be used.

An installer can be created by using the `Installer_win32.nsi` and
`Installer_x64.nsi` scripts in the Installer directory. This will require the
Nullsoft Scriptable Install System (NSIS) to be installed. Creating an
installer is not necessary to run Dolphin since the Build directory contains
a working Dolphin distribution.

## Oculus Rift
To build with Oculus Rift support, you need the Oculus Rift SDK 0.4.2 or above.
Copy the LibOVR folder from the Oculus Rift SDK into the Externals folder,
and clean and rebuild. The presense or absense of the LibOVR folder changes the
HAVE_OCULUSSDK preprocessor definition.
Currently only the OpenGL renderer works with the Oculus Rift.
Oculus Rift features are not officially supported by the Dolphin team.

## Installation on Linux and OS X
Dolphin requires [CMake](http://www.cmake.org/) for systems other than Windows. Many libraries are
bundled with Dolphin and used if they're not installed on your system. CMake
will inform you if a bundled library is used or if you need to install any
missing packages yourself.

### Build Steps:
1. `mkdir Build`
2. `cd Build`
3. `cmake ..`
4. `make`

On OS X, an application bundle will be created in `./Binaries`.

On Linux, it's strongly recommended to perform a global installation via `sudo make install`.

## Installation on Android
Dolphin requires [Android Studio](http://developer.android.com/tools/studio/index.html) to build
the Android UI. Import the Gradle project located in `./Source/Android`, and then execute the
Gradle task `assembleDebug` to build, or `installDebug` to install the UI onto a connected device.

In order to launch the app, you must build and include the native Dolphin libraries into the UI project.
Building native code requires the [Android NDK](https://developer.android.com/tools/sdk/ndk/index.html).

### Build Steps:
1. `mkdir Build-Android-<abi>`
2. `cd Build-Android-<abi>`
3. `cmake -DANDROID=True -DANDROID_NDK=<ndk-path> -DANDROID_NATIVE_API_LEVEL=android-18 -DANDROID_TOOLCHAIN_NAME=<toolchain> -DANDROID_ABI=<abi> -DCMAKE_TOOLCHAIN_FILE=../Source/Android/android.toolchain.cmake -DGIT_EXECUTABLE=<git-path> ..`
4. `make`

Replace `<git-path>` with the absolute path to your machine's Git executable, <ndk-path> with the absolute
path to where you installed your NDK, and the rest depending on which platform the Android device you are
targeting uses:

|Platform                 | abi         | toolchain                 |
|-------------------------|-------------|---------------------------|
|ARM 32-bit (most devices)| armeabi-v7a | arm-linux-androideabi-4.9 |
|ARM 64-bit (i.e. Nexus 9)| arm64-v8a   | aarch64-linux-android-4.9 |
|Intel 64-bit             | x86_64      | x86_64-4.9                |

The native libraries will be compiled, and copied into `./Source/Android/app/libs`. Android Studio
and Gradle will include any libraries in that folder into the APK at build time.

## Uninstalling
When Dolphin has been installed with the NSIS installer, you can uninstall
Dolphin like any other Windows application.

Linux users can run `cat install_manifest | xargs -d '\n' rm` from the build directory
to uninstall Dolphin from their system.

OS X users can simply delete Dolphin.app to uninstall it.

Additionally, you'll want to remove the global user directory (see below to
see where it's stored) if you don't plan to reinstall Dolphin.

## Command Line Usage
`Usage: Dolphin [-h] [-d] [-l] [-e <str>] [-b] [-V <str>] [-A <str>]`  

* -h, --help Show this help message  
* -d, --debugger Opens the debugger  
* -l, --logger Opens the logger  
* -e, --exec=<str> Loads the specified file (DOL,ELF,WAD,GCM,ISO)  
* -b, --batch Exit Dolphin with emulator  
* -V, --video_backend=<str> Specify a video backend  
* -A, --audio_emulation=<str> Low level (LLE) or high level (HLE) audio  

Available DSP emulation engines are HLE (High Level Emulation) and
LLE (Low Level Emulation). HLE is fast but often less accurate while LLE is
slow but close to perfect. Note that LLE has two submodes (Interpreter and
Recompiler), which cannot be selected from the command line.

Available video backends are "D3D" (only available on Windows) and
"OGL". There's also "Software Renderer", which uses the CPU for rendering and
is intended for debugging purposes only.

## Sys Files
* `totaldb.dsy`: Database of symbols (for devs only)
* `GC/font_ansi.bin`: font dumps
* `GC/font_sjis.bin`: font dumps
* `GC/dsp_coef.bin`: DSP dumps
* `GC/dsp_rom.bin`: DSP dumps

The DSP dumps included with Dolphin have been written from scratch and do not
contain any copyrighted material. They should work for most purposes, however
some games implement copy protection by checksumming the dumps. You will need
to dump the DSP files from a console and replace the default dumps if you want
to fix those issues.

## Folder Structure
These folders are installed read-only and should not be changed:

* `GameSettings`: per-game default settings database
* `GC`: DSP and font dumps
* `Maps`: symbol tables (dev only)
* `Shaders`: post-processing shaders
* `Themes`: icon themes for GUI
* `Resources`: icons that are theme-agnostic
* `Wii`: default Wii NAND contents

## User Folder Structure
A number of user writeable directories are created for caching purposes or for
allowing the user to edit their contents. On OS X and Linux these folders are
stored in `~/Library/Application Support/Dolphin/` and `~/.dolphin-emu`
respectively. On Windows the user directory is stored in the `My Documents`
folder by default, but there are various way to override this behavior:

* Creating a file called `portable.txt` next to the Dolphin executable will
  store the user directory in a local directory called "User" next to the
  Dolphin executable.
* If the registry string value `LocalUserConfig` exists in
  `HKEY_CURRENT_USER/Software/Dolphin Emulator` and has the value **1**,
  Dolphin will always start in portable mode.
* If the registry string value `UserConfigPath` exists in
  `HKEY_CURRENT_USER/Software/Dolphin Emulator`, the user folders will be
  stored in the directory given by that string. The other two methods will be
  prioritized over this setting.


List of user folders:

* `Cache`: used to cache the ISO list
* `Config`: configuration files
* `Dump`: anything dumped from Dolphin
* `GameConfig`: additional settings to be applied per-game
* `GC`: memory cards and system BIOS
* `Load`: custom textures
* `Logs`: logs, if enabled
* `ScreenShots`: screenshots taken via Dolphin
* `StateSaves`: save states
* `Wii`: Wii NAND contents

## Custom Textures
Custom textures have to be placed in the user directory under
`Load/Textures/[GameID]/`. You can find the Game ID by right-clicking a game
in the ISO list and selecting "ISO Properties".
