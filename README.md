## 我的字幕空间

我做字幕的地方
- 字幕成品发在 SubHD: https://subhd.tv/u/夏洛克聂
- 熟肉发布在B站: https://space.bilibili.com/6486757
- 或者阿里云盘: https://www.alipan.com/s/GJ5CHHy93mG

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

