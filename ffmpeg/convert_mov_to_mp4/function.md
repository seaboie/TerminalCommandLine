# Function

```bash
converttomp4() {
  # วิธีใช้:    converttomp4 input.mov                     # ได้ input_hevc.mp4 คุณภาพ 60
  #          converttomp4 input.mov [output.mp4]        # ตั้งชื่อผลลัพธ์เอง
  #          converttomp4 input.mov [output.mp4] [70]   # ค่า Default = 60
  if [[ -z "$1" ]]; then
    echo "วิธีใช้: converttomp4 input [output.mp4] [quality 1-100]"
    echo "quality : default = 60 >> ค่ายิ่งมาก ภาพยิ่งชัด"
    return 1
  fi

  local input="$1"
  local output="${2:-${input:r}_hevc.mp4}"
  local quality="${3:-60}"

  if [[ ! -f "$input" ]]; then
    echo "ไม่พบไฟล์: $input"
    return 1
  fi

  ffmpeg -n -i "$input" \
    -c:v hevc_videotoolbox -q:v "$quality" -tag:v hvc1 \
    -c:a aac -b:a 192k \
    -movflags +faststart \
    "$output"
}
```

