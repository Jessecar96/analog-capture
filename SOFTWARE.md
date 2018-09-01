## Required software
### FFmpeg
Required for all methods, you must install this to encode or capture any video.
1. Download ffmpeg from [here](https://ffmpeg.zeranoe.com/builds/). Use numerical version (like 4.0.2), 64-bit, and Static.
2. Extract the files from `ffmpeg-xxxxxx-win64-static/bin` to `C:\bin\ffmpeg`. You should have `C:\bin\ffmpeg\ffmpeg.exe` now.
3. In the start menu, search for `environment variables` and choose "Edit the system environment variables".
4. Click the "Environment Variables" button at the bottom of this window.
5. Under System Variables, find `Path` and double click it.
6. Click New on the right, and enter `C:\bin\ffmpeg`
7. Press OK on all windows to close them.
8. Done! Now you can use ffmpeg from the command prompt or powershell anyhwere on your system.

### Avidemux
Great to have, easy software to cut your video after capturing
Download and install from [http://avidemux.sourceforge.net](http://avidemux.sourceforge.net/download.html). Use the win64 build

### AmaRecTV and HuffYUV
Great alternative capture software. Only required for methods 2 and 3.
This only records in raw AVI and does not do any encoding. FFmpeg is required to encode or deinterlace your video.
I also reccomend installing the HuffYUV codec as it reduces the disk space required to record raw AVI.

1. Download AmaRecTV from [http://www.amarectv.com](http://www.amarectv.com/english/amarectv_e.htm) and extract it to a folder of your choice.
2. Download the HuffYUV codec from [here](https://www.videohelp.com/download/huffyuv64.zip) and extract the 2 files to `C:\huffyuv64`
3. In the start menu, search for `powershell` then right click it and Run as administrator.
4. Run `cd C:\Windows\SysWOW64\`
5. Run `rundll32.exe setupapi.dll,InstallHinfSection DefaultInstall 0 C:\huffyuv64\huffyuv.inf` to install HuffYUV
6. Run `AmaRecTV.exe` then press the first button at the top for Config.
7. Under General, change the folder you want to record to.
8. Under Graph 1(Device) select your video and audio capture device. Use 720x480, YUY2, and 29.97 fps
9. Under Graph 2(Preview) select 4:3 aspect ratio and "Not use" for deinterlacing
10. Under recording, click "Other Codec" and "Update Codec List". Then select HFYU from the list.
11. Click OK to close.

### AviSynth+ and QTGMC
AviSynth is a very powerful tool to process and edit video. This is only needed for method 3.
1. Download and install AviSynth+ from [http://avs-plus.net](http://avs-plus.net/). Use the reccomended install options.
2. Download QTGMC [here](https://forum.doom9.org/attachment.php?attachmentid=16429&d=1531449747). Extract it to `C:\Program Files (x86)\AviSynth+\plugins64+`
3. Download MaskTools2 [here](https://github.com/pinterf/masktools/releases). Extract the contents of `x64` to `C:\Program Files (x86)\AviSynth+\plugins64+`
4. Download MvTools2 [here](https://github.com/pinterf/mvtools/releases). Extract the contents of `x64` to `C:\Program Files (x86)\AviSynth+\plugins64+`
5. Download NNEDI3 [here](https://github.com/jpsdr/NNEDI3/releases). Extract the contents of `x64\Release_W7_AVX2` to `C:\Program Files (x86)\AviSynth+\plugins64+`
6. Download RgTools [here](https://github.com/pinterf/RgTools/releases). Extract the contents of `x64` to `C:\Program Files (x86)\AviSynth+\plugins64+`
7. Open SMDegrain [here](https://pastebin.com/raw/u1xsPLwK) and save it as `SMDegrain.avsi` into `C:\Program Files (x86)\AviSynth+\plugins64+`
8. Download PlanarTools [here](https://github.com/chikuzen/PlanarTools/releases). Extract the contents of `x64` to `C:\Program Files (x86)\AviSynth+\plugins64+`
9. Download Cnr2 [here](https://www.dropbox.com/s/9fiab3s5exi43h2/cnr2_v261-avs26.zip?dl=1). Extract `CNR2_x64.dll` to `C:\Program Files (x86)\AviSynth+\plugins64+`
10. Download WarpSharp [here](https://github.com/jpsdr/aWarpSharpMT/releases). Extract the contents of `x64\Release_W7_AVX2` to `C:\Program Files (x86)\AviSynth+\plugins64+`
9. You're done, finally!
