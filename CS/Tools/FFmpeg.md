B站循环直播视频
```python
import os, time

command = 'ffmpeg -re -i "video.mp4" -vcodec copy -acodec aac -b:a 192k -f flv "rtmp://live-push.bilivideo.com/live-bvc/live-dl/?streamname=live_3493115000261231_60500665&key=4eb064553e16c2bb58a6fa12d0f8243b&schedule=rtmp&pflag=1"'

while True:
    os.system(command)
    time.sleep(5)
```