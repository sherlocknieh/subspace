
## FFmpeg 用法笔记

- 安装 FFmpeg
```
# Win11:
winget install ffmpeg               #安装到默认目录
winget install ffmpeg -l C:/Apps    #安装到指定目录
```

- FFmpeg 命令基本格式
```
ffmpeg [输入参数] -i "输入文件" [输出参数] "输出文件.后缀名"
```

## 压字幕

- 压5分钟样片
```
ffmpeg -i "视频.mkv" -vf "ass='字幕.ass'" -c:v h264_qsv -q:v 20 -c:a copy -t 600 "输出.mp4"
```

- 参数解释:

```
 -vf "ass='字幕.ass'"       # ASS字幕
 -vf "subtitles='字幕.srt'" # SRT字幕
 -c:v h264_qsv              # GPU加速  [使用 Intel 核显 QSV 编解码器]
 -q:v 20                    # 视频质量 [默认值26质量一般, 20质量较高, 18接近无损]
 -c:a copy                  # 音频直接复制
 -ss 00:00:00               # 起始时间
 -t 300                     # 片段长度: 300秒 (5分钟)
```

GPU加速模式下, 只能使用"恒定质量"编码, 即每帧保持相同的质量水平。

无法使用更为灵活的 CRF 模式(根据帧复杂度动态调整);

但是速度比CPU编码快5倍, 能减少CPU发热, 防止风扇狂转;

把任务切到后台可进一步减少CPU发热。



- 解码过程也可以使用GPU加速

```powershell
ffmpeg -hwaccel qsv -c:v h264_qsv -i "视频.mkv" -vf "hwdownload,format=nv12,ass='字幕.ass'" -c:v h264_qsv -q 20 -c:a copy "输出.mp4"
```

GPU解码输出的qsv格式与ASS字幕滤镜不兼容, 所以需要添加格式转换步骤: hwdownload,format=nv12


其它显卡和编码格式
```
# 输入选项
 -hwaccel qsv               # Intel核显
 -hwaccel cuda              # NVIDIA显卡
 -hwaccel d3d11va           # AMD显卡

 -c:v h264_qsv              # Intel核显, H.264编码
 -c:v hevc_qsv              # Intel核显, HEVC编码
 -c:v av1_qsv               # Intel核显, AV1编码

 -c:v h264_nvenc            # NVIDIA显卡, H.264编码
 -c:v hevc_nvenc            # NVIDIA显卡, HEVC编码
 -c:v av1_nvenc             # NVIDIA显卡, AV1编码
```


## 视频切片

```powershell
ffmpeg -i "输入.mp4" -c copy -f segment -segment_time 180 -reset_timestamps 1 -segment_start_number 1 "P%02d.mp4"
```

参数解释:
```
 -c copy                    # 无损切割
 -f segment                 # 分段输出
 -segment_time 180          # 每段180秒 (3分钟)
 -reset_timestamps 1        # 重置时间戳 (每段从0开始)
 -segment_start_number 1    # 分段起始编号设为1
 "P%02d.mp4"                # 输出文件名格式 (P01.mp4)
```


