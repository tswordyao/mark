## mac程序运行 Chrome
```
Chrome 自动播放视频
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome  ~/Desktop/test.mp4

Chrome 自动加载网页
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome https://www.google.com

如果chrome是缺省浏览器， open http://www.zhihu.com
如果不是 ,  open -a "/Applications/Google Chrome.app" 'http://www.zhihu.com'
————————————————
 --enable-usermedia-screen-capturing
 --no-sandbox 
 --user-data-dir
 ```

##  mac一些手动处理脚本
 ```
 # vscode终端卡顿处理
codesign --remove-signature /Applications/Visual\ Studio\ Code.app/Contents/Frameworks/Code\ Helper\ \(Renderer\).app

# 清除右键打开方式选项
/System/Library/Frameworks/CoreServices.framework/Frameworks/LaunchServices.framework/Support/lsregister -kill -r -domain local-domain system -domain user

/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -kill -r -domain local -domain system-domainuser
```
