# How to capture, deinterlace, and encode analog video

## Introduction
I've spent about 3 years now trying various methods to get the best captures of old analog video such as VHS and Video8/Hi8 tapes. A lot of what I've read describes ways to do this for old TVs, DVDs, or old computers which just isn't going to cut it in 2018.
I have 3 methods of capturing and encoding. I personally use a mix of the 3 depending on what I'm capturing.
For a capture device I use a Hauppauge USB-Live 2 on Windows 10 64-bit, so these instructions will be based on that.

### Interlacing
Analog video is interlaced. Interlacing means each frame of video is split into 2 parts, each called a field. This video is captured and played back at 59.94 fields per second, or 29.97 frames per second for NTSC video. Analog CRT TVs play back interlaced video, but all modern LCD/LED displays only display progressive video. To play interlaced video on a progressive display the video must be deinterlaced or else you'll see jagged lines anywhere there's motion, [like this](https://upload.wikimedia.org/wikipedia/commons/1/19/Interlaced_video_frame_%28car_wheel%29.jpg).

If you're going to be playing your video on modern displays or upload it to the web you should deinterlace your video! You *can* leave it interlaced and your video player or TV will deinterlace it for you, but most TVs have poor deinterlacing methods. You can use some software to do a much better job.

### Encoding
In order to save your video it must be encoded into a video codec. Some examples are H.264, H.265, and VP9. A lot of older guides and some capture software encode video in MPEG2 which is just outdated and doesn't perform nearly as well as H.264.

## Method 1

**Use FFmpeg to capture, deinterlace, and encode your video all at once.**
This method uses little storage space, and doesn't require any 2nd processing steps.

1. Choose a folder you want to save your video to, and hold down shift and right click.
2. Choose "Open PowerShell window here"
3. List your capture devices with this command:
```
ffmpeg -list_devices true -f dshow -i dummy
```
4. Find the audio and video device that cooresponds to your capture device. Mine are `Hauppauge Cx23100 Video Capture` and `Hauppauge Cx23100 Audio Capture`.
5. Use this command to record your video. Make sure to substitute in your capture devices.
```
ffmpeg -f dshow -video_size 720x480 -framerate 29.97 -pixel_format yuyv422 -i video="Hauppauge Cx23100 Video Capture":audio="Hauppauge Cx23100 Audio Capture" -c:v libx264 -crf 18 -aspect 4:3 -vf "yadif=1" -pix_fmt yuv420p -c:a aac -b:a 392k recording.mp4
```
6. Running this command will start capturing right away. To stop press `q`. Since you cannot see a preview I reccomend testing at least once before capturing the full thing.
### Explanation
- `-f dshow` Use the DirectShow input format, for capture devices on Windows.
- `-video_size 720x480` Set our video side to 720x480, for NTSC video. USe 720x576 for PAL video.
- `-framerate 29.97` Use 29.97 FPS, for NTSC video. Use 25 for PAL video
- `-pixel_format yuyv422` Capture in YUYV 4:2:2
- `-i video="Hauppauge Cx23100 Video Capture":audio="Hauppauge Cx23100 Audio Capture"` Set our input sources
- `-c:v libx264` Set our encoder to x264, which encodes to H.264.
- `-crf 18` Set out output video quality. Lower is better/higher bitrate. See [this](https://slhck.info/video/2017/02/24/crf-guide.html) for more deails on CRF.
- `-aspect 4:3` Set our aspect ratio to 4:3, which mostly all analog video uses.
- `-vf "yadif=1"` Use the yadif deinterlacer and use interpolation to generate 59.94 fps video. You can use `-vf "yadif=0"` instead to generate 29.97 fps video if you don't like the more fluid motion.
- `-pix_fmt yuv420p` Set our output format to YUYV 4:2:0, which is required for most video players.
- `-c:a aac` Set our audio encoder to AAC
- `-b:a 384k` Set our audio bitrate to 384 kilobits/sec
- `recording.mp4` Set the output filename of our recording


## Method 2
**Use AmaRecTV to capture, and FFmpeg to encode and deinterlace**
This method uses signifcant disk space as it records to uncompressed AVI. It also takes more time since it requires a 2nd step to encode it to a more efficient format.

1. Open AmaRecTV and verify your video shows up fine.
2. Click the play button to record, and stop when you're done.
3. You can use the mute and unmute button to monitor your audio. This does not affect your recording.
4. Optionally use avidemux to cut the beging and end off your video. Make sure to use "Copy" for both the video and audio formats, and save as AVI.
5. Use ffmpeg to encode your uncompressed video to H.264 video. Make sure to change the input file name.
```
ffmpeg -i 'amarec(XXXXXXXXXX).avi' -c:v libx264 -crf 18 -aspect 4:3 -vf "yadif=1" -pix_fmt yuv420p -c:a aac -b:a 384k output.mp4
```
The only difference here from above is using `-i filename.avi` instead of DirectShow for the input.

## Method 3
**Use AmaRecTV to capture, AviSynth and QTGMC to deinterlace, and FFmpeg to encode**
This method takes signifcant disk space and signifcant time to process and encode the video, but it produces the best results.

1. Open AmaRecTV and verify your video shows up fine.
2. Click the play button to record, and stop when you're done.
3. You can use the mute and unmute button to monitor your audio. This does not affect your recording.
4. Optionally use avidemux to cut the beging and end off your video. Make sure to use "Copy" for both the video and audio formats, and save as AVI.
5. Create a text file and save it as `deinterlace.avs` with this content:
```
import("C:\Program Files (x86)\AviSynth+\plugins64+\MT.avs")
AviSource("amarec(XXXXXXXX-XXXX).avi").ConvertToYV12() # Enter your file here
AssumeTFF() # Use top field first
QTGMC(Preset="Slow") # Deinterlace
Cnr2() # Reduce chroma noise
DeHalo_alpha() # De-halo
Prefetch(8) # Use 8 threads, enter your CPU thread count here!
```
6. Change your input file name in AviSource, and enter your CPU's thread count in Prefetch.
7. Now use FFmpeg to process this script and encode the video:
```
ffmpeg -i 'deinterlace.avs' -c:v libx264 -crf 18 -aspect 4:3 -pix_fmt yuv420p -c:a aac -b:a 384k output.mp4
```
This is the same as method 2, but we're omitting the yadif deinterlacer and changing the input file name.
