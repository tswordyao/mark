# ffmpeg
参数在最下面 ↓↓↓
```
把avi转成微信能直接播放的mp4
ffmpeg  -i bys.avi -bsf:a aac_adtstoasc -codec copy bys.mp4
ffmpeg  -i 1214540019.avi -bsf:a aac_adtstoasc -codec copy 1214540019.mp4

抓取在线视频为本地mp4
fmpeg  -i "https://vod.cn/sd.m3u8" -bsf:a aac_adtstoasc -codec copy 1214511649.mp4

ffmpeg  -i "http://jdvodluwytr3t.vod.126.net/jdvodluwytr3t/nos/hls/2020/02/05/1215872154_b70c2c018c694051b9b1d45205640c0d_shd.m3u8" -bsf:a aac_adtstoasc -codec copy cshd.mp4

处理图片
 ffmpeg -hide_banner -i  solar.png -pix_fmt pal8  solar2.png 
ffmpeg -hide_banner -i  solar3.png -pix_fmt pal8 -s 1280x800  solar03.png 


ffmpeg -ss 0 -t 84 -accurate_seek -i bys.avi -codec copy bys.mp4

精确剪切视频，从0到360秒
ffmpeg -ss 0 -t 360 -accurate_seek -i 801.mp4 -codec copy 19801.mp4
ffmpeg -ss 0 -t 360 -accurate_seek -i 801.mp4 -codec copy 19801.mp4

如果编码格式采用的copy 最好加上 -avoid_negative_ts 1参数
ffmpeg -ss 50 -t  77 -accurate_seek -i 20190824_142007.MP4 -codec copy  -avoid_negative_ts 1  hj.mp4

抓取视频某帧为图片http://trac.ffmpeg.org/wiki/Seeking
ffmpeg -ss 00:23:00 -i Mononoke.Hime.mkv -frames:v 1 out1.jpg

ffmpeg -i "网址" -c copy  my.avi

ffmpeg -i "http://upos-hz-mirrorcosu.acgvideo.com/upgcxcode/61/59/7945961/7945961-1-48.mp4" -c copy bb.avi

ffmpeg -i "https://vdn.vzuu.com/LD/88807e44-e53c-11e8-b810-0242ac112a22.mp4?auth_key=1542373083-0-0-d74b889417447bc62c708a571c7405ca&expiration=1542373083&v=ali&bu=com&f=mp4&disable_local_cache=1" -c copy shushi.avi

ffmpeg -i "https://www.bilibili.com/0e7429df-2bc1-4c62-8918-a7ce9b449be2" -c copy dragon.avi


<div class="ux-video-player" id="auto-id-1537968412764">

    
<object classid="clsid:d27cdb6e-ae6d-11cf-96b8-444553540000" codebase="http://fpdownload.macromedia.com/get/flashplayer/current/swflash.cab" width="100%" height="100%" id="_1537968412763">    <param value="//edu-image.nosdn.127.net/component-video-player-swf-eduPlayer_be877349a3014ffd45d3be0ba2633556.swf" name="movie">        <param value="transparent" name="wmode">        <param value="always" name="allowscriptaccess">        <param value="true" name="allowFullScreen">        <param value="namespace=edu.front.flashVideoPlayer50&amp;host=http://study.163.com&amp;logId=undefined&amp;isLive=false&amp;isAutoPlay=true&amp;isPreload=true&amp;volume=0.8&amp;isLocal=false&amp;showCdnSwitch=true&amp;isInnerSite=true&amp;showPauseAd=false&amp;waterMark=false&amp;uiRemotePath=//edu-image.nosdn.127.net/component-video-player-swf-ui-cloudPlayerUI_bea90e41cfd162b6c65eddc5b77b6776.swf&amp;id=_1537968412763" name="flashvars">        <embed src="//edu-image.nosdn.127.net/component-video-player-swf-eduPlayer_be877349a3014ffd45d3be0ba2633556.swf" name="_1537968412763" width="100%" height="100%" pluginspage="http://www.adobe.com/go/getflashplayer" type="application/x-shockwave-flash" wmode="transparent" allowscriptaccess="always" allowfullscreen="true" flashvars="namespace=edu.front.flashVideoPlayer50&amp;host=http://study.163.com&amp;logId=undefined&amp;isLive=false&amp;isAutoPlay=true&amp;isPreload=true&amp;volume=0.8&amp;isLocal=false&amp;showCdnSwitch=true&amp;isInnerSite=true&amp;showPauseAd=false&amp;waterMark=false&amp;uiRemotePath=//edu-image.nosdn.127.net/component-video-player-swf-ui-cloudPlayerUI_bea90e41cfd162b6c65eddc5b77b6776.swf&amp;id=_1537968412763"></object></div>


youtube-dl 和 ffmpeg 配合下载B站视频
￼
Ross_Yang 关注


有些B站的视频是多节拼接的，直接下载以后是多个flv的片段，eg：
youtube-dl -F https://www.bilibili.com/video/av10641220
downloading playlist

然后下载这13个片段
youtube-dl -f 1 https://www.bilibili.com/video/av10641220
Downloading....


下载完成以后记得重命名为01-13.flv，便于下一步操作。

先转换为ts格式；
for i in {01..13};do ffmpeg -i $i.flv -c copy -bsf:v h264_mp4toannexb -f mpegts $i.ts;done

进行合并即可
ffmpeg -i "concat:01.ts|02.ts|03.ts|04.ts|05.ts|06.ts|07.ts|08.ts|09.ts|10.ts|11.ts|12.ts|13.ts" -c copy -bsf:a aac_adtstoasc -movflags +faststart 01.mp4


ffmpeg -f  concat -i   list.txt  -c copy  out.mp4

contents of 1.txt:
file '01.mp4'
file '02.mp4'
file '03.mp4'

下载工具: idm(internet download manager)官网
```





