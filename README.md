# ffmpeg reference
Commonly used ffmpeg commands in my work

## Get latest ffmpeg (Linux x86_64)
```bash
cd ~/temp
wget "https://johnvansickle.com/ffmpeg/builds/ffmpeg-git-64bit-static.tar.xz"
tar -xJf ffmpeg-git-64bit-static.tar.xz
mv ffmpeg-git-*/ffmpeg ~/bin/
```

## General ffmpeg command structure
```bash
ffmpeg [input_parameters] -i <input_file | input_files_pattern> \
       -c:v <video_codec> [video_codec_options] \
       [-vf "<video_filter0, video_filter1, ...>"] \
       [-an | -c:a <audio_codec> [audio_codec_options]] \
       [other_video_options] \
       <output_video_name>
```

## Creating mp4 video
Currently the most popular format - used by most journals
https://trac.ffmpeg.org/wiki/Encode/H.264

|option|value|
|--|--|
| video_codec | libx264 |
| video_codec_options | -crf 18 | 

### CRF
https://trac.ffmpeg.org/wiki/Encode/H.264#crf

Range 0-51: where 0 is lossless, 23 is default, and 51 is worst possible.

A lower value is a higher quality and a subjectively sane range is 18-28.

Consider 18 to be visually lossless or nearly so: it should look the same or nearly the same as the input but it isn't technically lossless.

### Example: Convert cine file to mp4 played at 5 FPS
#### Lossless
```bash
ffmpeg -r 5 -i input.cine -c:v libx264 -crf 0 -an -pix_fmt yuv420p output.mp4
```
Change -crf from 0 to 18 for "visually lossless" output video. Note that output video will have a bit depth of 8.

