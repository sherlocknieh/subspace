## 我的字幕空间

我做字幕的地方
- 字幕成品发在 SubHD: https://subhd.tv/u/夏洛克聂
- 熟肉发布在B站: https://space.bilibili.com/6486757

## FFmpeg命令

- 压字幕 (-t 600 压5分钟样片, 删掉 -t 600 压全片)

```
Intel 核显:

ffmpeg -i "视频.mkv" -vf "subtitles='字幕.ass'" -c:v h264_qsv -q:v 18 -c:a copy -t 600 "输出.mp4"

NVIDIA 显卡:

ffmpeg -i "视频.mkv" -vf "subtitles='字幕.ass'" -c:v h264_nvenc -q:v 18 -c:a copy -t 600 "输出.mp4"
```




- 宽屏电影, 先上下加黑边, 再加字幕
```
ffmpeg -i "视频.mkv" -vf "pad=iw:iw*9/16:(ow-iw)/2:(oh-ih)/2:black, subtitles='字幕.ass'" -c:v h264_qsv -q:v 20 -c:a copy "输出.mp4"
```

- 切片 (切成多个3分钟小片段, 会自动找关键帧位置切, 所以时长会在3分钟上下浮动)

```
ffmpeg -i "视频.mp4" -c copy -f segment -segment_time 180 -reset_timestamps 1 -segment_start_number 1 "P%02d.mp4"
```

- [详细](/FFmpeg.md)

## ASS字幕设置

```
中文字体: 思源黑体 Medium, 字号:80, 边框: 3, 阴影: 0,  垂直边距: 20

{\fnArial\fs40\bord2}   # 英文字体: Arial, 字号: 40, 边框: 2

\blur4                  # 边框模糊度: 4
\an3                    # 靠边位置: 右上角
\pos(640,36)            # 精准定位: (640,36)
\fad(400,100)           # 淡入: 400ms, 淡出: 100ms

\1c&0xFFFFFF            # 字体颜色: 白色
\3c&0x000000            # 边框颜色: 黑色

```

- [详细](/ASS.md)