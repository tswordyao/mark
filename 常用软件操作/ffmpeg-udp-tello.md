```
#ffmpeg接收udp流, 转换成RTMP视频再推流

ffmpeg -i udp://192.168.10.1:8889 -vcodec copy -acodec aac -ar 44100 -strict -2 -ac 1 -f flv -s 960*540 -q 10 rtmp推流地址

使用mpv播放
mpv udp://0.0.0.0:11111 --no-cache --untimed --no-demuxer-thread --video-sync=audio --vd-lavc-threads=1

抓取知乎视频

ffmpeg -i https://XXXX/video.m3u8 -c copy  output.mp4

ffmpeg -i "https://vdn.vzuu.com/SD/4bc7b6.mp4" -c copy kawayi-game.avi


ffmpeg -i   1009765012.mp4 -b:v 360k 1009765012_sd.mp4

# 压缩视频
ffmpeg -i  xxx.MP4 -b:v 1440k yyy.mp4

ffmpeg -i "udp://127.0.0.1:8001" -c copy drones.avi

ffmpeg -f h264 -i udp://0.0.0.0:11111  output-zonde.avi

ffmpeg -i  219.MP4 -b:v 1440k 20.mp4

直接播放udp流
ffplay udp://233.233.233.233:6666

原文：https://blog.csdn.net/xdwyyan/article/details/44957125 

tello
架构
建立特洛 Tello 和 PC、Mac 或移动设备之间的 Wi-Fi 通信

发送命令和接收响应
Tello IP：192.168.10.1 UDP PORT：8889 << - - >> PC / Mac / Mobile
备注1：在PC，Mac或移动设备上设置UDP客户端，向特洛 Tello UDP 端口 8889 发送命令和接收响应。
备注2：在发送所有其他命令之前，向特洛 Tello UDP 端口 8889 发送“command” 命令以启动特洛 Tello 的 SDK 模式。


接收特洛 Tello 状态
Tello IP：192.168.10.1 - >> PC / Mac / Mobile UDP Server：0.0.0.0 UDP PORT：8890
备注3：在 PC，Mac 或移动设备上建立 UDP 服务器，通过 UDP 端口 8890 从 IP 0.0.0.0收听消息。如果未进行备注1和2的操作，请先完成。


接收特洛 Tello 视频流
Tello IP：192.168.10.1 - >> PC / Mac / Mobile UDP Server：0.0.0.0 UDP PORT：11111
备注4：在 PC，Mac 或移动设备上设置 UDP 服务器，通过服务器 UDP 端口 11111 从IP 0.0.0.0 收听消息。

备注5：先进行备注1和2的操作。然后向特洛 Tello UDP 端口 8889 发送 “streamon” 命令，开始接受特洛 Tello 视频流。

* 设置命令
* 控制命令
* 读取命令
* Tello状态
命令	描述	可能的响应
speed?	获取当前设置速度
（cm/s）	x
x = (10-100)
battery?	获取当前电池剩余
电量的百分比值	x
x = (0-100)
time?	获取电机运转时间时间
（s）	x
height?	获取相对高度 (cm)	x: 10-3000
temp?	获取主板
最高和最低温度(℃)	x: 0-90
attitude?	获取 IMU 三轴姿态数据	pitch roll yaw
pitch=（-89°- 89°）
roll=（-179°- 179°）
yaw=（-179°- 179°）
baro?	获取气压计高度(m)	x
acceleration?	获取 IMU 三轴加速度数据(0.001g)	x y z
tof?	获取 ToF 的高度值(cm)	x: 10-400 & 6553
返回 6553 意味着测量值超过
ToF 量程。
wifi?	获得 Wi-Fi 信噪比	snr
```