

```bash
ffmpeg -i img.mov -c:v hevc_videotoolbox -q:v 68 -c:a aac -b:a 128k -tag:v hvc1 -vf scale=1280:720 img_68_1280_720.mp4
```

> `scale=1280:720`


