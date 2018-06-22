# Audio library for The Powder Toy
The "first true implementation"(?) of audio in TPT, within TPT itself, without the use of external programs. Uses an external library (SDL) instead, which could be easily gathered. SDL is cross-platform, so it can (probably) work anywhere TPT is on (as long as it has LuaJIT support, and SDL libraries are available).

This kind-of simple script uses LuaJIT's FFI library to load the SDL library. [SDL](https://www.libsdl.org/index.php) stands for Simple DirectMedia Layer, which is a cross-platform library that provides access to graphics, audio, input, and et cetera. It's a C library, meaning you cannot load it with Lua `require()`. The FFI library does the magic, though it needs a header file for it to work (See [`SDL-FFI.h`](audio/SDL-FFI.h)).

## Requirements
* TPT version 92.2 (build 333) and up. (Which supports LuaJIT)
* SDL2 and SDL_mixer libraries, versions >=2.0.0 (Instructions to get these libraries below.)

## Installation
### Windows
1. Clone this repository somewhere on your computer.
2. Place the `audio` folder from this repo into your Powder installation folder. __Place the folder, not the files within it.__
3. Download the SDL libraries.
   - For SDL2, navigate [here](https://www.libsdl.org/download-2.0.php) and download the 32-bit Windows runtime binary [(direct link)](https://www.libsdl.org/release/SDL2-2.0.8-win32-x86.zip). __Always__ choose 32-bit (x86) regardless of your computer type, as TPT is 32-bit.
   - For SDL_mixer, get it [here](https://www.libsdl.org/projects/SDL_mixer/). Again, 32-bit Windows runtime binaries [(direct link)](https://www.libsdl.org/projects/SDL_mixer/release/SDL2_mixer-2.0.2-win32-x86.zip).
4. Move the .dll files beside Powder.exe, Only `SDL2.dll` and `SDL2_mixer.dll` are required from the packages you downloaded.
5. You should be able to use it now. See Usage section below.

### Mac OS X (untested)
1. Do steps 1 and 2 from the Windows installation instructions.
3. Download the SDL libraries.
   - For SDL2, navigate [here](https://www.libsdl.org/download-2.0.php) and download the Mac OS X runtime binary [(direct link)](https://www.libsdl.org/release/SDL2-2.0.8.dmg).
   - For SDL_mixer, get it [here](https://www.libsdl.org/projects/SDL_mixer/) [(direct link)](https://www.libsdl.org/projects/SDL_mixer/release/SDL2_mixer-2.0.2.dmg).
4. Open each of the .dmg files, and move both `SDL2.framework` and `SDL2_mixer.framework` to `/Library/Frameworks` on your computer. You could place it on the user frameworks folder instead if you cannot do that thing.
5. You should be able to use it now. See Usage section below.

### Linux
(note: tested only on Ubuntu 16.04)
1. Do steps 1 and 2 from the Windows installation instructions.
3. Gather the SDL libraries. Make sure you run commands as root, or `sudo` them. (note, this adds 32-bit libraries into your 64-bit machine.)
   - __For Debian/Ubuntu-based machines__: (Last line is optional, do that if you want to only have 64-bit repo lists.)
     ```
     dpkg --add-architecture i386
     apt-get update
     apt-get install libsdl2-2.0-0:i386 libsdl2-mixer-2.0-0:i386
     dpkg --remove-architecture i386
     ```
   
   - __With `pacman`__: 
     - uncomment the `[multilib]` section in `/etc/pacman.conf` (both lines).
       ```
       [multilib]
       Include = /etc/pacman.d/mirrorlist
       ```
     - then do:
       ```
       pacman -Syu
       pacman -S lib32-sdl2 lib32-sdl2_mixer
       ```

   - __Other package managers__: search for `libSDL2` and `libSDL2_mixer` __32-bit__ libraries, for it to work on TPT.
4. You should be able to use it now. See Usage section below.

## Usage
Using the library within Lua is pretty simple...
```lua
audio = require("audio.audio)
```
Load a WAV file: (I might add in support for other audio formats soon.)
```lua
fwee = audio.load("Fwee.wav")  -- Fwee.wav is the file to load.
```
Then play it:
```lua
audio.play(fwee)
```
This library uses channels for playing audio, and one audio can play per channel at a time. By default, the library allocates 8 channels, but you can increase it by setting the global variable `MIXER_CHANS` before `require`-ing the library.
The `audio.play` usage above defaults to the next available channel (second argument is -1 by default). The function also returns the channel number the audio is played, which you can use to set effects like panning and volume.
```lua
channel = audio.play(fwee)
audio.setVolume(channel, 64)  -- half output volume
```
View [`audio.lua`](audio/audio.lua) for the available functions you can use.

## To-do:
From v0.8

* Support for other audio formats, e.g. OGG, MP4, etc.
* Add built-in effects that SDL provides.
* OOP structures?
* Maybe use SDL's functions for loading music? 

