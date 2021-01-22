FFmpeg

## 压缩图片 ImageOptim
```
 ffmpeg -i in.png out.png
 ffmpeg -i in.png -pix_fmt pal8 -y out.png
 ffmpeg -i in.png  -vf scale=iw/2:ih/2 out.png
 和压缩视频一样: 
ffmpeg -i input.mp4 -vf scale=960:540 output.mp4 
如果540不写，写成-1，即scale=960:-1, 那也是可以的，保持原始宽高比。
```

## 批量处理
$ for f in ./img/*.jpg; do ffmpeg -i $f -q:v 10 outimg/$f; done


## 裁切参数
crop=100:100:12:34
crop=w=100:h=100:x=12:y=34


## 转m3u8
ffmpeg -i   1007428102_blue.mp4 -c:v libx264 -c:a aac -strict -2 -f hls  -hls_time 60 output.m3u8

## 拉m3u8
ffmpeg -i "xxx.m3u8" -c copy xxx.avi

## 编码不支持的需要试试这个
```
ffmpeg  -i "https://jdvodrvfb210d.vod.126.net/jdvodrvfb210d/nos/hls/2020/02/25/1216048698_94e0601b43ec473a97a3b9e972631752_sd.m3u8" -bsf:a aac_adtstoasc -codec copy 20000words-1-2.mp4

ffmpeg  -i "陈正康-英语两万词.avi" -bsf:a aac_adtstoasc -codec copy 20000words-1-1.mp4

ownerId= 1407535961  陈正康-英语两万词
scrach videoId 1007410044 yooc courseId 2001284002
```

## 合并视频
ffmpeg -i "concat:01.ts|02.ts|03.ts" -c copy -bsf:a aac_adtstoasc -movflags +faststart 123.mp4

## 压缩视频
```
ffmpeg -i  1010789127_hd.mp4 -b:v 1440k 1010789127_sd.mp4
ffmpeg -i video-1008007039.mp4 -s 960x540 1008007039_sd.mp4
ffmpeg -i  1010789127_hd.mp4 -s 960x540 1010789127_sd.mp4
ffmpeg -i  hd.mp4 -b:v  480k  sd.mp4
```

## 也可以用来修复
ffmpeg -i  ui326.mp4 -b:v  720k  ui326-0.mp4

## 添加图片水印 
```
ffmpeg -i 1217144666.mp4 -vf "movie=water-study.png[watermark];[in][watermark] overlay=main_w-overlay_w-10:main_h-overlay_h-10[out] " 1217144666-w.mp4   #右下角

ffmpeg -i 1217144666.mp4 -vf "movie=water-study.png[watermark];[in][watermark] overlay=main_w-overlay_w-10:y=10[out] " 1217144666-w.mp4   #左上角

ffmpeg -i input.mp4 -vf "drawtext=fontfile=simhei.ttf: text="技术":x=10:y=10:fontsize=24:fontcolor=white:shadowy=2" output.mp4 #左上角添加白色文字水印

ffmpeg -i 1217146900.mp4 -vf "movie=water-study.png[watermark];[in][watermark] overlay=main_w-overlay_w-10:-10[out] " 1217146900-w.mp4   #右上角
```

## 查看视频信息
```
ffprobe -hide_banner -v quiet -print_format json -show_format -show_streams in.png > in.log

ffprobe -hide_banner -v quiet -print_format json -show_format -show_streams in.mp4 > in.log

ffprobe -hide_banner -v quiet -print_format json -show_format -show_streams 3f2d70bd-8750-4c65-8b8c-ac49bc18446e.mp4 > in.log
```

## 【FFmpeg】FFmpeg常用基本命令
```
0. 抽取视频某一帧为图片
ffmpeg -i 913.mp4 -threads 1 -ss 00:02:50 -f image2 -r 1 -t 1 -s 1280:720 pic.jpeg
-r 1表示只抽取一帧, 实际上每秒有很多帧

1.分离视频音频流
ffmpeg -i input_file -vcodec copy -an output_file_video　　//分离视频流
ffmpeg -i input_file -acodec copy -vn output_file_audio　　//分离音频流

2.视频解复用
ffmpeg –i test.mp4 –vcodec copy –an –f m4v test.264
ffmpeg –i test.avi –vcodec copy –an –f m4v test.264

3.视频转码
ffmpeg –i test.mp4 –vcodec h264 –s 352*278 –an –f m4v test.264              //转码为码流原始文件
ffmpeg –i test.mp4 –vcodec h264 –bf 0 –g 25 –s 352*278 –an –f m4v test.264  //转码为码流原始文件
ffmpeg –i test.avi -vcodec mpeg4 –vtag xvid –qsame test_xvid.avi            //转码为封装文件
//-bf B帧数目控制，-g 关键帧间隔控制，-s 分辨率控制

4.视频封装
ffmpeg –i video_file –i audio_file –vcodec copy –acodec copy output_file

5.视频剪切
ffmpeg –i test.avi –r 1 –f image2 image-%3d.jpeg        //提取图片
ffmpeg -ss 0:1:30 -t 0:0:20 -i input.avi -vcodec copy -acodec copy output.avi    //剪切视频
//-r 提取图像的频率，-ss 开始时间，-t 持续时间

ffmpeg -ss 0:0:02 -t 0:0:48 -i 913.MP4 -vcodec h264 -vf scale=960:540 output.mp4

ffmpeg -ss 0:59:30 -t 2:11:0 -i 801458742989160448.mp4 -vcodec copy -acodec copy new.mp4

6.视频录制
ffmpeg –i rtsp://192.168.3.205:5555/test –vcodec copy out.avi

7.YUV序列播放
ffplay -f rawvideo -video_size 1920x1080 input.yuv

8.YUV序列转AVI
ffmpeg –s w*h –pix_fmt yuv420p –i input.yuv –vcodec mpeg4 output.avi

常用参数说明：
主要参数： -i 设定输入流 -f 设定输出格式 -ss 开始时间 视频参数： -b 设定视频流量，默认为200Kbit/s -r 设定帧速率，默认为25 -s 设定画面的宽与高 -aspect 设定画面的比例 -vn 不处理视频 -vcodec 设定视频编解码器，未设定时则使用与输入流相同的编解码器 音频参数： -ar 设定采样率 -ac 设定声音的Channel数 -acodec 设定声音编解码器，未设定时则使用与输入流相同的编解码器 -an 不处理音频
```

------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------

```
0.压缩转码mp4文件
ffmpeg -i input.avi -s 640x480 output.avi
ffmpeg -i input.avi -strict -2 -s vga output.avi
 
1、将文件当做直播送至live
ffmpeg -re -i localFile.mp4 -c copy -f flv rtmp://server/live/streamName
2、将直播媒体保存至本地文件
 
ffmpeg -i rtmp://server/live/streamName -c copy dump.flv
3、将其中一个直播流，视频改用h264压缩，音频不变，送至另外一个直播服务流
 
ffmpeg -i rtmp://server/live/originalStream -c:a copy -c:v libx264 -vpre slow -f flv rtmp://server/live/h264Stream
 
4、将其中一个直播流，视频改用h264压缩，音频改用faac压缩，送至另外一个直播服务流
ffmpeg -i rtmp://server/live/originalStream -c:a libfaac -ar 44100 -ab 48k -c:v libx264 -vpre slow -vpre baseline -f flv rtmp://server/live/h264Stream

5、将其中一个直播流，视频不变，音频改用faac压缩，送至另外一个直播服务流
ffmpeg -i rtmp://server/live/originalStream -acodec libfaac -ar 44100 -ab 48k -vcodec copy -f flv rtmp://server/live/h264_AAC_Stream

6、将一个高清流，复制为几个不同视频清晰度的流重新发布，其中音频不变
ffmpeg -re -i rtmp://server/live/high_FMLE_stream -acodec copy -vcodec x264lib -s 640×360 -b 500k -vpre medium -vpre baseline rtmp://server/live/baseline_500k -acodec copy -vcodec x264lib -s 480×272 -b 300k -vpre medium -vpre baseline rtmp://server/live/baseline_300k -acodec copy -vcodec x264lib -s 320×200 -b 150k -vpre medium -vpre baseline rtmp://server/live/baseline_150k -acodec libfaac -vn -ab 48k rtmp://server/live/audio_only_AAC_48k

7、功能一样，只是采用-x264opts选项
ffmpeg -re -i rtmp://server/live/high_FMLE_stream -c:a copy -c:v x264lib -s 640×360 -x264opts bitrate=500:profile=baseline:preset=slow rtmp://server/live/baseline_500k -c:a copy -c:v x264lib -s 480×272 -x264opts bitrate=300:profile=baseline:preset=slow rtmp://server/live/baseline_300k -c:a copy -c:v x264lib -s 320×200 -x264opts bitrate=150:profile=baseline:preset=slow rtmp://server/live/baseline_150k -c:a libfaac -vn -b:a 48k rtmp://server/live/audio_only_AAC_48k

8、将当前摄像头及音频通过DSSHOW采集，视频h264、音频faac压缩后发布
ffmpeg -r 25 -f dshow -s 640×480 -i video=”video source name”:audio=”audio source name” -vcodec libx264 -b 600k -vpre slow -acodec libfaac -ab 128k -f flv rtmp://server/application/stream_name

9、将一个JPG图片经过h264压缩循环输出为mp4视频
ffmpeg.exe -i INPUT.jpg -an -vcodec libx264 -coder 1 -flags +loop -cmp +chroma -subq 10 -qcomp 0.6 -qmin 10 -qmax 51 -qdiff 4 -flags2 +dct8x8 -trellis 2 -partitions +parti8x8+parti4x4 -crf 24 -threads 0 -r 25 -g 25 -y OUTPUT.mp4

10、将普通流视频改用h264压缩，音频不变，送至高清流服务(新版本FMS live=1)
ffmpeg -i rtmp://server/live/originalStream -c:a copy -c:v libx264 -vpre slow -f flv “rtmp://server/live/h264Stream live=1〃

```
------------------------------------------------------------------------
----------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------
 
```
1.采集usb摄像头视频命令：
ffmpeg -t 20 -f vfwcap -i 0 -r 8 -f mp4 cap1111.mp4
 
./ffmpeg -t 10 -f vfwcap -i 0 -r 8 -f mp4 cap.mp4
具体说明如下：我们采集10秒，采集设备为vfwcap类型设备，第0个vfwcap采集设备（如果系统有多个vfw的视频采集设备，可以通过-i num来选择），每秒8帧，输出方式为文件，格式为mp4。
 
2.最简单的抓屏：
ffmpeg -f gdigrab -i desktop out.mpg 
 
3.从屏幕的（10,20）点处开始，抓取640x480的屏幕，设定帧率为5 ：
ffmpeg -f gdigrab -framerate 5 -offset_x 10 -offset_y 20 -video_size 640x480 -i desktop out.mpg 
 
4.ffmpeg从视频中生成gif图片：
ffmpeg -i capx.mp4 -t 10 -s 320x240 -pix_fmt rgb24 jidu1.gif
 
5.ffmpeg将图片转换为视频：
http://blog.sina.com.cn/s/blog_40d73279010113c2.html
```

## ffmpeg推流
```
ffmpeg -re -i a.mp4 -vcodec h264 -acodec aac -strict -2 -f flv "rtmp://p03f9fed7.live.126.net/live/fbde045e38a94009ba016d59d71994ba?wsSecret=eb530dffaf80f70f0cb383db61a2959c&wsTime=1555642029"

ffmpeg -f avfoundation   -framerate 30 -i  0 -vcodec h264 -acodec aac -strict -2 -f flv "rtmp://p03f9fed7.live.126.net/live/fbde045e38a94009ba016d59d71994ba?wsSecret=eb530dffaf80f70f0cb383db61a2959c&wsTime=1555642029"

 ffmpeg -f avfoundation  -pix_fmt uyvy422    -video_size 640x480  -framerate 30 -i  0 -vcodec h264 -acodec aac -strict -2 -f flv "rtmp://p03f9fed7.live.126.net/live/fbde045e38a94009ba016d59d71994ba?wsSecret=eb530dffaf80f70f0cb383db61a2959c&wsTime=1555642029"


$ find ./ -name '*.mp4' -exec sh -c 'ffmpeg -i "$0" -c:v libx264 -crf 30 -c:a aac "${0%%.mp4}.small.mp4"' {} \;
将当前文件夹中的所有 .mp4 后缀名的文件，压缩到新文件 文件名.small.mp4 中
```

### ffmpeg参数
```
a) 通用选项

-L license
-h 帮助
-fromats 显示可用的格式，编解码的，协议的...
-f fmt 强迫采用格式fmt
-I filename 输入文件
-y 覆盖输出文件
-t duration 设置纪录时间 hh:mm:ss[.xxx]格式的记录时间也支持
-ss position 搜索到指定的时间 [-]hh:mm:ss[.xxx]的格式也支持
-title string 设置标题-author string 设置作者-copyright string 设置版权-comment string 设置评论
-target type 设置目标文件类型(vcd,svcd,dvd) 所有的格式选项（比特率，编解码以及缓冲区大小）自动设置，只需要输入如下的就可以了：ffmpeg -i myfile.avi -target vcd /tmp/vcd.mpg
-hq 激活高质量设置
-itsoffset offset 设置以秒为基准的时间偏移，该选项影响所有后面的输入文件。该偏移被加到输入文件的时戳，定义一个正偏移意味着相应的流被延迟了 offset秒。 [-]hh:mm:ss[.xxx]的格式也支持

b) 视频选项

-b bitrate 设置比特率，缺省200kb/s   提示用-b:a 或-b:v
-r fps 设置帧频 缺省25
-s size 设置帧大小 格式为WXH 缺省160X128.下面的简写也可以直接使用：
Sqcif 128X96 qcif 176X144 cif 252X288 4cif 704X576
-aspect aspect 设置横纵比 4:3 16:9 或 1.3333 1.7777
-croptop size 设置顶部切除带大小 像素单位
-cropbottom size –cropleft size –cropright size
-padtop size 设置顶部补齐的大小 像素单位
-padbottom size –padleft size –padright size –padcolor color 设置补齐条颜色(hex,6个16进制的数，红:绿:兰排列，比如 000000代表黑色)
-vn 不做视频记录
-bt tolerance 设置视频码率容忍度kbit/s
-maxrate bitrate设置最大视频码率容忍度
-minrate bitreate 设置最小视频码率容忍度
-bufsize size 设置码率控制缓冲区大小
-vcodec codec 强制使用codec编解码方式。如果用copy表示原始编解码数据必须被拷贝。
-sameq 使用同样视频质量作为源（VBR）
-pass n 选择处理遍数（1或者2）。两遍编码非常有用。第一遍生成统计信息，第二遍生成精确的请求的码率
-passlogfile file 选择两遍的纪录文件名为file

c)高级视频选项

-g gop_size 设置图像组大小
-intra 仅适用帧内编码
-qscale q 使用固定的视频量化标度(VBR)
-qmin q 最小视频量化标度(VBR)
-qmax q 最大视频量化标度(VBR)
-qdiff q 量化标度间最大偏差 (VBR)
-qblur blur 视频量化标度柔化(VBR)
-qcomp compression 视频量化标度压缩(VBR)
-rc_init_cplx complexity 一遍编码的初始复杂度
-b_qfactor factor 在p和b帧间的qp因子
-i_qfactor factor 在p和i帧间的qp因子
-b_qoffset offset 在p和b帧间的qp偏差
-i_qoffset offset 在p和i帧间的qp偏差
-rc_eq equation 设置码率控制方程 默认tex^qComp
-rc_override override 特定间隔下的速率控制重载
-me method 设置运动估计的方法 可用方法有 zero phods log x1 epzs(缺省) full
-dct_algo algo 设置dct的算法 可用的有 0 FF_DCT_AUTO 缺省的DCT 1 FF_DCT_FASTINT 2 FF_DCT_INT 3 FF_DCT_MMX 4 FF_DCT_MLIB 5 FF_DCT_ALTIVEC
-idct_algo algo 设置idct算法。可用的有 0 FF_IDCT_AUTO 缺省的IDCT 1 FF_IDCT_INT 2 FF_IDCT_SIMPLE 3 FF_IDCT_SIMPLEMMX 4 FF_IDCT_LIBMPEG2MMX 5 FF_IDCT_PS2 6 FF_IDCT_MLIB 7 FF_IDCT_ARM 8 FF_IDCT_ALTIVEC 9 FF_IDCT_SH4 10 FF_IDCT_SIMPLEARM
-er n 设置错误残留为n 1 FF_ER_CAREFULL 缺省 2 FF_ER_COMPLIANT 3 FF_ER_AGGRESSIVE 4 FF_ER_VERY_AGGRESSIVE
-ec bit_mask 设置错误掩蔽为bit_mask,该值为如下值的位掩码 1 FF_EC_GUESS_MVS (default=enabled) 2 FF_EC_DEBLOCK (default=enabled)
-bf frames 使用frames B 帧，支持mpeg1,mpeg2,mpeg4
-mbd mode 宏块决策 0 FF_MB_DECISION_SIMPLE 使用mb_cmp 1 FF_MB_DECISION_BITS 2 FF_MB_DECISION_RD
-4mv 使用4个运动矢量 仅用于mpeg4
-part 使用数据划分 仅用于mpeg4
-bug param 绕过没有被自动监测到编码器的问题
-strict strictness 跟标准的严格性
-aic 使能高级帧内编码 h263+
-umv 使能无限运动矢量 h263+
-deinterlace 不采用交织方法
-interlace 强迫交织法编码仅对mpeg2和mpeg4有效。当你的输入是交织的并且你想要保持交织以最小图像损失的时候采用该选项。可选的方法是不交织，但是损失更大
-psnr 计算压缩帧的psnr
-vstats 输出视频编码统计到vstats_hhmmss.log
-vhook module 插入视频处理模块 module 包括了模块名和参数，用空格分开

D)音频选项

-ab bitrate 设置音频码率
-ar freq 设置音频采样率
-ac channels 设置通道 缺省为1
-an 不使能音频纪录
-acodec codec 使用codec编解码

E)音频/视频捕获选项

-vd device 设置视频捕获设备。比如/dev/video0
-vc channel 设置视频捕获通道 DV1394专用
-tvstd standard 设置电视标准 NTSC PAL(SECAM)
-dv1394 设置DV1394捕获
-av device 设置音频设备 比如/dev/dsp

F)高级选项

-map file:stream 设置输入流映射
-debug 打印特定调试信息
-benchmark 为基准测试加入时间
-hex 倾倒每一个输入包
-bitexact 仅使用位精确算法 用于编解码测试
-ps size 设置包大小，以bits为单位
-re 以本地帧频读数据，主要用于模拟捕获设备
-loop 循环输入流（只工作于图像流，用于ffserver测试）

详细的使用说明（英文）：http://ffmpeg.org/ffmpeg.html


ffmpeg手册上说，多个http-headers设置是采用CRLF来进行分割的
如果需要将User-Agent and X-Forwarde采用CRLF进行拼接，合成一个参数，应该采用如下格式：
-header "User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/46.0.2490.80 Safari/537.36"$'\r\n'"X-Forwarded-For: 13.14.15.66"$'\r\n'
```


