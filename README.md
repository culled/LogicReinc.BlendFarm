# **BlendFarm**
A open-source, cross-platform, stand-alone, Network Renderer for Blender
---
When I was trying to build a render server I was suprised most network renderers out there for Blender are either outdated, obsolete or require very specific environments to work properly. Thus, I spend the last months writing and testing a stand-alone network renderer that requires barely any setup and should work with most if not all recent versions of Blender. And should even work with future releases. 

Originally I only planned on using it for myself, but decided to make it more production ready and release it to the public, and hopefully solve this for others. 
It consumed more time than I had planned. But as we're in a time where any modern GPU is effectively unobtainium, there is not a better time for it to exist.

![SimpleRender](https://raw.githubusercontent.com/LogicReinc/LogicReinc.BlendFarm/master/.resources/example.gif)
A render using SplitChunked render strategy on BlendFarms test blendfile at low samples/res for showcase.

## Why a Network Renderer
----
Not everyone has RTX 3090's stacked up, and even if you do, you can only run maybe 2 in the same system. A network renderer allows you to use multiple pcs in your network to work on a single image or animation. Old hardware that is still relatively fast can be used to accelerate your rendering or live preview. 
While a network renderer is not perfect and does have some overhead. With 2 identical computers on a sufficiently large scene I estimate you can speed up your render time by about 80-90% with the right settings.

##  TL;DR: Features
----
Watch the video below for an overview:

[![Demonstration Video](https://img.youtube.com/vi/J5fYMfi6Nb0/0.jpg)](https://www.youtube.com/watch?v=J5fYMfi6Nb0)
 * **Distributed Rendering** - Duh, Network renderer. Support both CPU and GPU (CUDA/OPENCL, OPTIX planned when I obtain Ampere)
 * **Stand-alone client** - Automatically updates when your .blend updates, doesn't slow Blender
 * **Headless server** - Easy deployment with a single executable
 * **Blender Versions** - Automatically downloads the right version of Blender
 * **Render Images in Chunks** - Supports different rendering strategies including those showing rendered chunks
 * **Live Update** - Re-Render automatically when you save your project
 * **Auto Discovery** - Application will attempt to automatically detect nodes in your network
 * **Automatic Performance** - Measures performance after each render to improve distribution (todo improve)
 * **Queueed Rendering** - Queue up multiple renders with specific configurations

## Why Stand-alone
----
Unlike some other network renderers, LogicReinc.BlendFarm runs as its own independent application. This is for various reasons:
 - **Cross-Blender Version** - Allows for easy switching between Blender versions without changing software
 - **No Hassle with Preferences** - Your preferences are not edited by the application, It only copies and reads the files.
 - **More Future Proof** - failure is now concentrated into a single small script, other logic is safe
 - **Automatic Installation** - No need to install versions of Blender, All handled by the application when versions are required
 - **Improved Workflow** - not very different if not better with live re-render
 - **Responsive** - Doesn't slow down Blender. Python is also extremely slow in comparison.
 - **Stable** - Doesn't cause Blender to crash on odd failures, You don't want to lose that project.


## Use-Cases
 * **Old Hardware** - Recently upgraded from your still powerful system? Let it render
 * **GPU Limit** - You can't fit a GPU into your existing system? Let it render in another computer
 * **Powerfull side Laptop** - Got a powerful laptop with a good GPU? Let it render
 * **Cheap Chinese Hardware** - Get some cheap xeon systems from China and build your own render farm
---
## Downloads
https://github.com/LogicReinc/LogicReinc.BlendFarm/releases
___
## If you like the work, please consider a donation

Quite some time went into getting this up. If you enjoy the software, please consider dropping a donation on my Patreon.
https://patreon.com/LogicReinc

If you like to donate crypto:

Bitcoin: 3FidXvMfvqusmXzKroCLZ2gUE1YMNVVHoz

Ethereum: 0xd2C3BCCc981d359e037457f0CdB9d48fdc289Feb

---
## Planned
 - Batch Animation Frames (Reduce "Render Loading.. between frames)
 - Auto-merge animations into video/gifs
 - Render Focus (target a specific area to render first)
 - Render Rectangle (just render a specific area)
 - Settings Interface
 - Improve Automatic Performance
 - General Improvement
 - Docker Image?
 - More Testing

## Limitations
-----
### Overhead
The software is not perfect, depending on the Render Strategy and blend file there will be overhead, but with the right settings additional hardware should be about 90% effective. Performance will likely improve overtime as well with features such as GUI taking over CPU tiles in Blender 2.92.0. **For the fastest rendering use RenderStrategy Splitted**, this does not show your image being rendered in chunks but in whole pieces per device. But has less overhead.
### Permissions (Linux)
On Linux in particular, the application will need read, write and execute permissions in a directory. Unless you want to run it with sudo.
### Memory Usage
Its still using Blender underneath, and thus you need to cough up the memory you actually need for the files.
___
# Showcase

### CPU Speed Comparison
Below a simple example when rendering in split mode, using all computers and a single computer
1700X + 3700X + Xeon e5 2578 v3 + 2x Xeon e5 2573 v3 | Solo 1700X
:------------------------:|:----------------------:
![SimpleRender](https://raw.githubusercontent.com/LogicReinc/LogicReinc.BlendFarm/master/.resources/renderSplitted.gif) | ![SimpleRender](https://raw.githubusercontent.com/LogicReinc/LogicReinc.BlendFarm/master/.resources/renderSolo.gif)
Duration: +/- 25 Seconds | Duration: +/- 90 Seconds
With a well distributed scene performance scaling can be >90%

### Animation Rendering
Using BlendFarm you can render multiple frames at the same time.
![AnimationRender](https://raw.githubusercontent.com/LogicReinc/LogicReinc.BlendFarm/master/.resources/renderAnimation.gif)

### Live Render
You can run BlendFarm besides Blender, and have it re-render your scene using all your connected computers whenever you save in blender
![LiveRender](https://raw.githubusercontent.com/LogicReinc/LogicReinc.BlendFarm/master/.resources/renderLive.gif)

 
___

## Source Code Notes
----
 * **UI Code needs improvement** - Was experimenting with Avalonia, lot of it is unorganized.
 * **Unit Tests are basic** - Unit Tests have to be run individually and need to be improved in general.
 * **Memory Management** - There are some places especially in the TCP protocol that can have minor memory improvements. But seems like overoptimalization at the moment.
___

# Technical Information
----
* **Language (C# .Net Core 3.0, Avalonia UI)**
The application is written entirely in C# under .Net Core 3.0, with Avalonia as cross-platform UI Framework. A small amount of Python is present for the Blender communication.

* **Networking (Custom TCP Protocol, UDP Broadcasts)**
Communication is done over a handmade TCP protocol, WebSocket was just too slow for syncing files (from 10 to <1 second for 200mb .blend file).
Discovery is done through UDP broadcasts.

___
# Getting Started
----
To get started, You will want 2 or more computers (if you actually want to distribute it).

Below is a video on how you do a basic render using BlendFarm

[![BlendFarm Tutorial](https://img.youtube.com/vi/EXdwD5t53wc/0.jpg)](https://www.youtube.com/watch?v=EXdwD5t53wc)


### Platform Specific TL;DR; 
Extract and run the executables, ensure read, write and execute access to directory and allow through firewall (port 15000 default)
Note that LogicReinc.BlendFarm is both client and server, so you can simply use the GUI on every pc, but would add useless UI.

### Getting Started on Windows

#### Server (Render node)
 - Download the latest LogicReinc.BlendFarm.Server release at https://github.com/LogicReinc/LogicReinc.BlendFarm/releases (LogicReinc.BlendFarm.Server-Win64)
 - Extract and Run LogicReinc.BlendFarm.Server.exe to any directory with read/write access.
 - When asked, allow the application through your firewall

#### Client (GUI)
 - Download the latest LogicReinc.BlendFarm release at https://github.com/LogicReinc/LogicReinc.BlendFarm/releases (LogicReinc.BlendFarm-Win64)
 - Extract and Run LogicReinc.BlendFarm.exe to any directory with read/write access.
 - When asked, allow the application through your firewall
 
### Getting Started on Linux

#### Server (Render Node)
 - Download the latest LogicReinc.BlendFarm.Server release at https://github.com/LogicReinc/LogicReinc.BlendFarm/releases (LogicReinc.BlendFarm.Server-Linux64)
 - Extract LogicReinc.BlendFarm.Server to any directory with read/write/execute access (chown/chmod u=rwx, or sudo)
 - (Optional: if firewall) Allow port 15000/tcp (default, can change) and port 16342/udp (default, can change) through your firewall
 - Run LogicReinc.BlendFarm.Server

#### Client (GUI)
 - Download the latest LogicReinc.BlendFarm release at https://github.com/LogicReinc/LogicReinc.BlendFarm/releases (LogicReinc.BlendFarm-Linux64)
 - Extract LogicReinc.BlendFarm to any directory with read/write/execute access (chown/chmod u=rwx, or sudo)
 - (Optional: if firewall) Allow port 15000/tcp (default, can change) and port 16342/udp (default, can change) through your firewall
 - Run LogicReinc.BlendFarm

### Getting Started on Mac

#### Client (GUI)
 - Download the latest LogicReinc.BlendFarm release at https://github.com/LogicReinc/LogicReinc.BlendFarm/releases (LogicReinc.BlendFarm-OSX64)
 - Extract LogicReinc.BlendFarm-OSX64 to any directory with read/write
 - (Optional: if firewall) Allow port 15000/tcp (default, can change) and port 16342/udp (default, can change) through your firewall
 - Run LogicReinc.BlendFarm.command

### Finding my RenderNodes
After you setup your computers render nodes should automatically appear in your nodes list through auto-discovery.
If this is not the case, your network may block broadcasts. You can add nodes by IP using the fields under the list.
For address use {deviceIP}:{devicePort} (eg. 192.168.1.123:15000)

___

## Tested Blender Versions (Server)
 - blender-2.83.9-windows64
 - blender-2.83.9-linux64
 - blender-2-92.0-windows64
 - blender-2.92.0-linux64
 - blender-2-92.0-macOS
 - blender-2.91.0-windows64
 - blender-2.91.0-linux64
 - blender-2.91.2-windows64
 - blender-2.91.2-linux64
 - blender-2.91.2-macOS
 - blender-2.92.5-windows64
 - blender-2.93.5-linux64
 
___

# FAQ

#### Which version of Blender does this work with?
In theory, this tool can be made to work with all (recent) versions of Blender. Even future blender versions may work auto-magically unless Blender's source code changes significantly. In this case I will have to add updated scripts.
You select which version of Blender to render with while when opening your project/blend file.

#### Can I use GPU rendering?
Yup, when connected to a client you can click the gear and change RenderType to CUDA or OPENCL

#### Can I run this on Windows?
Yes, both the renderer and gui should work on Windows64. 

#### Can I run this on Linux?
Yes, both the renderer and gui should work on Linux64. 

#### Can I run this on Mac?
Yes, both the renderer and gui should work on OSX-64, however may not run on ARM CPUs. (At the moment only 64bit blender versions are used) 

#### Can I run this in Docker?
I haven't made a docker file yet, but I plan on it since its not that much effort. So expect it in the future Iguess.
If you want to make one yourself, you can easily do it by copying the linux executable, assign a persistent volume to its parent directory and run it on boot and you're good. When I make the docker file I'll make some changes to the code so that environment variables can be used instead of Settings file.

#### Can I run this on Linux headless?
Yes, the render node server is a console application and has no GUI. 

#### Can I donate for your work?
Every donation is welcome through my Patreon https://patreon.com/LogicReinc. No tiers or rewards at the moment. Just for donations. All current software is available freely.

#### Render Animation gives a set of images, how do I get a video?
Currently I have not embeded ffmpeg into the application, you can easily merge it using ffmpeg command line. In a later version I may embed ffmpeg to automatically generate mkv/mp4/etc.

#### Do I need to port forward?
Assuming you're running this over a local network, there is no reason

#### Can I run this over the internet?
Sure, In that case you do need port forwarding, I do not recommend exposing the renderer on the internet cuz there is no security, private VPN maybe?
Also note that syncronizing will be slower.

#### The scene is rendered without textures or similar?
This is likely due to missing texture files. Make sure you pack your textures into your .blend file (File->External Data->Automatically Pack into .blend).
This ensures remote computers have the textures.

#### My Blenderfile requires script to run during rendering, can these be run?
At this point in time Blendfarm will not execute scripts in your Blendfile as this would be a security concern. I may add something so you can explicitly enable it somehow at a later point. (While I do not recommend it, create an issue if you really need this)

#### My Blendfile requires special addons to be active while rendering, can I add these?
Blendfarm has its own versions of Blender in the BlenderData directory, and it runs these versions always in factory startup, thus without any added addons. This is done on purpose to make sure the environment is not altered. Most addons don't have to be active during rendering as they generate geometry etc.
If you really need this, make an issue and I see what I can do. However do realise that this may make the workflow less smooth. (As you may need to set up these plugins for every Blender version instead of just letting BlendFarm do all the work.

#### Automatic Performance doesn't distribute work equally
The software tries to equally distribute it based on past renders, there might be run-to-run variation that can cause it to be off.
Also keep in mind that automatic performance is only applied after completing a renderin (so not cancelled).
This is an area to improve. 

#### My internet is slow, downloading the Blender version for every pc takes too long
In theory you can download the zip/tar.xz from blender directly and extract is into AppDirectory/BlenderData (unless you changed it in settings). Just ensure you use the right naming format, generally blender-x.xx.x-osversion64, for example blender-2.91.0-windows64, and in case of linux ensure you chmod -R u=rwx that directory

#### I fail to connect to another computer
The most likely reason connection failed is simply a firewall on the target pc. If you can ping the computer and the firewall is allowed for the used port there should be no general connection issues.

#### Render fails due to Gdip
You're running Linux or Mac but did not install libgdiplus and libc6-dev, install these and you should be good.

#### Render fails on Linux
You may not have the required blender system dependencies. Easiest way to cover them all is to just run apt-get install blender to fetch them all.
(It does not have to be an up2date blender package, its just for dependencies)

#### I'm seeing artifacts in the end result
Depending on your settings, there can be a variety of reasons for this. In general, if you're rendering for an actual export. I recommend using Render Strategy Split, this doesn't give you a nice live preview of progress, but it does render a single chunk for every device, this generally causes favorable results.
When using strategy SplitChunked with Workaround enabled, this generally causes line to appear due to rounding issues.
In summary, use Split for final renders and SplitChunked for live previews

#### Chunks are rendered in the wrong location
Try toggling the Workaround checkbox. In versions of blender (atleast 2.91.0 and below), persistent renders don't work together with changing of render settings, which this software relies on, in this case Workaround should be ON. If this causes the tiles to render in the wrong location, turn it OFF. It may be fixed in that version of Blender.

#### Each chunk is shown surrounded by black, but they are never merged
In some projects the background around render borders is rendered in black, I believe this has something to do with compositing. I still have to figure that one out and is on my TODO list.

#### Syncing takes too long
This highly depends on your project and local network connection. Blend files can get very big. There is little I can do about that.
Make sure computers are connected by cable and not over wifi.

#### It is rendering slower than it should
There could be a variety of reasons why rendering is slow, and most of them you can likely fix with the settings.
 
 - **I'm using CUDA/OPENCL/GPU**
 Keep in mind that when you use CUDA, you use both CPU and GPU, In Blender 2.91 and below GPU cannot take over CPU tiles, thus if there are not enough tiles to render, your GPU will be idle waiting for the CPU to finish. Reducing TileSize will allow the GPU to do more, but also slower.
  - **I'm using Render Strategy Chunked**
 When using Chunked, blender is initialized for every chunk, this can be extremely slow depending on your project. I suggest using SplitChunked, this may not allow perfect distribution of work, but it will allow each device to render everything within a single Blender instance.
 - **I'm using Render Strategy Split Chunked**
 Generally, SplitChunked is has minimal overhead depending on the project file. Try to avoid using compositing in your project atleast while using the program. It is executed for every chunk. Increasing the ChunkSize may also reduce overhead.
 
#### I have a different issue
You can create an issue on Github, please make sure you checked all your settings beforehand, and add as much details as possible in your Issue description. Steps you did to reproduce, etc.

