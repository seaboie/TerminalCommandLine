# Function. ปรับความเร็วของวีดีโอ
- **วิธีใช้**
> วิธีใช้ 1: converttomp4slowly input.mp4 

> วิธีใช้2 ( ระบุ อาร์กิวเมนต์ เพิ่ม ): converttomp4slowly input.mp4 [output.mp4] [speed 0.5-1.0] [quality 1-100]

> ค่า Default speed = 75

> ค่า Default quality = 60


```bash
converttomp4slowly() {
  # วิธีใช้: converttomp4slowly input.mp4  [output.mp4]  [speed 0.5-1.0]  [quality 1-100]
  if [[ -z "$1" ]]; then
    echo "วิธีใช้ 1: converttomp4slowly input.mp4 "
    echo "วิธีใช้2 ( ระบุ อาร์กิวเมนต์ เพิ่ม ): converttomp4slowly input.mp4 [output.mp4] [speed 0.5-1.0] [quality 1-100] "
    echo "speed : default = 0.75"
    echo "quality : default = 60 >> ค่ายิ่งมาก ภาพยิ่งชัด"
    return 1
  fi

  local input="$1"
  local output="${2:-${input:r}_hevc.mp4}"
  local speed="${3:-0.75}"
  local quality="${4:-60}"
  

  if [[ ! -f "$input" ]]; then
    echo "ไม่พบไฟล์: $input"
    return 1
  fi

  # เช็คว่ามีแทร็กเสียงไหม
  if ffprobe -v error -select_streams a -show_entries stream=index \
       -of csv=p=0 "$input" | grep -q .; then
    ffmpeg -n -i "$input" \
      -filter_complex "[0:v]setpts=PTS/${speed}[v];[0:a]atempo=${speed}[a]" \
      -map "[v]" -map "[a]" \
      -c:v hevc_videotoolbox -q:v "$quality" -tag:v hvc1 \
      -c:a aac -b:a 192k \
      -movflags +faststart \
      "$output"
  else
    ffmpeg -n -i "$input" \
      -filter:v "setpts=PTS/${speed}" -an \
      -c:v hevc_videotoolbox -q:v "$quality" -tag:v hvc1 \
      -movflags +faststart \
      "$output"
  fi
}
```
