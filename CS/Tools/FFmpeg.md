视频格式转换(在**macOS**上使用硬件加速)

```bash
ffmpeg -i input.mkv -acodec copy -c:v h264_videotoolbox -q:v 100 output.mp4
```