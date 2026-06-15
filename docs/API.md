在使用AI Agent开发时，可直接将本页url发给Agent来理解和编写代码
## setView("video1")
传入 canvas视图的id，当使用mse时，播放器自动转换为video标签

## setScaleMode(0)
视频缩放模式, 当视频分辨率比例与canvas显示区域比例不同时,缩放效果不同:
* 0 视频画面完全填充canvas区域,画面会被拉伸
* 1 视频画面做等比缩放后,高或宽对齐canvas区域,画面不被拉伸,但有黑边
* 2 视频画面做等比缩放后,完全填充canvas区域,画面不被拉伸,没有黑边,但画面显示不全

## setScaleLevel(x: number, y: number, level: number): void;
视频放大
   * @param x  视频放大点 x
   * @param y  视频放大点 y
   * @param level 缩放等级,从 1.00 ~ 0.00, 1.0就是原始画面大小, 值越小越放大画面


##   setCanvasScaleLevel(x: number, y: number, level: number): void; 
画布放大,可以根据监听click事件获取到鼠标点击的画布坐标
   * @param x 画布放大点x
   * @param y 画布放大点y
   * @param level 缩放等级,从 1.00 ~ 0.00, 1.0就是原始画面大小, 值越小越放大画面

## setBufferTime(500)
设置最大缓冲时长，单位毫秒，播放器会自动消除延迟，默认2000。
0.5.56及以后版本，当设置为0时，不进行视频缓存，在不考虑视频平滑度的情况下以绝对0延迟进行播放，音频以能达到的最低延迟进行播放。适用于对延迟要求非常高的场景，如5G远程操作。也适用于推流端网络掉包非常严重，音视频帧无法同步导致的画面停顿。
如果视频流按倍数传送过来，设置0也能达到倍数播放的效果

## setVolume(1.0)
设置音量大小，取值0.0 -- 1.0
当为0.0时，完全无声
当为1.0时，最大音量，默认值

## setTimeout(10)
设置超时时长, 单位秒
在连接成功之前和播放中途,如果超过设定时长无数据返回,则回调timeout事件

## setKeepScreenOn()
开启屏幕常亮, 在start前调用
在手机浏览器上, canvas标签渲染视频并不会像video标签那样保持屏幕常亮
H5目前在chrome\edge 84, android chrome 84及以上有原生亮屏API, 需要是https页面
其余平台为模拟实现，此时为兼容实现，并不保证所有浏览器都支持
>0.5.54及之后支持原生API屏幕常亮

## resizeView(640,360)
重新调整视图大小
640 -宽
360 -高
调用后播放器内部自动将canvas视图大小和css大小修改为设定值
注意：如果需要手动调整高度后也调用该方法，否则可能引起GL渲染尺寸改变影响画面效果
> v0.5.30版之后不再建议使用，请使用onResize

## onResize(viewRotate, bodyRotate = 0)
通知底层重新计算渲染尺寸
在播放过程中，如果业务层调整了视图大小，调用该方法通知底层重新计算渲染尺寸，修复可能引起的画面模糊。
* viewRotate - view旋转角度，默认0，可设90，270，旋转画面。
可用于实现监控画面小窗和全屏效果，由于iOS没有全屏API，此方法可以模拟页面内全屏效果而且多端效果一致。
* bodyRotate - body旋转角度，默认0，用于通过css将body整体旋转了的情况，可不传该参数

## setCryptoKey(key)
设置视频解密密码，长度16字节。可以实现SDK加密推流或NMS服务端加密后的解密播放

## screenshot(filename, format, quality)  
截图，调用后弹出下载框保存截图
filename: 保存的文件名 
format :  截图的格式，可选png或jpeg
quality: 可选参数，当格式是jpeg时，压缩质量，取值0.0 ~ 1.0
>v0.5.33后可以，手机浏览器不一定能保存下来。
>图片大小是canvas的画布大小，非css大小，也非视频大小。

## fullscreen()
全屏播放视频
>iOS不支持，iPadOS13后支持

## audioResume()
留给上层用户操作来触发音频恢复的方法。
iPhone，chrome等要求自动播放时，音频必须静音，需要由一个真实的用户交互操作来恢复，不能使用代码。
https://developers.google.com/web/updates/2017/09/autoplay-policy-changes
NodePlayer.js的视图框内监听了点击事件，当点击画面时可以恢复。
但如果上层开发覆盖了视图的点击区域，可以自行增加一个触发条件，调用这个方法来恢复。

## audioState()
获取当前音频状态, 返回值 closed, running, suspended


## NodePlayer.debug(false)
类方法
是否开启控制台调试打印，全局调用

## NodePlayer.activeAudioEngine()
类方法
使用active音频引擎

## NodePlayer.workletAudioEngine()
类方法
使用audioWorklet音频引擎

## useMSE()
自动测试浏览器是否支持MSE播放，如不支持，仍然使用软解码。
紧随 new 后调用，不调用则只使用软解。

## useWorker()
使用worker线程解码, 适用于多画面直播, 能有效利用多核处理器
>v0.8.0之后可用

## useWCS()
使用WebCodecs硬件解码，目前支持chrome/edge 86及以上版本，或使用chromium内核86(360浏览器13)及以上版本，
~~需打开实验功能来开启，浏览器地址栏输入 chrome://flags/#enable-experimental-web-platform-features 选择开启
或者将 --enable-blink-features=WebCodecs 作为浏览器启动参数~~
chrome或chrome内核94之后默认已开启
>v0.8.6之后可用

## start("http://demo.com/live/stream.flv")
开始播放视频

## stop()
停止播放视频

## pause(isPause )
暂停或恢复播放点播类视频

## startRecord(filename: string): number;
开始录像，传入保存的文件名，只能是.mp4后缀

## stopRecord(): void;
停止录像

## clearView()
清理画布为黑色背景
用于canvas重用进行多个流切换播放时，将上一个画面清理
避免后一个视频播放之前出现前一个视频最后一个画面
在stop后调用
>v0.5.46及以后版本

##  enableAudio(enable)
boolean类型参数
是否开启音频流处理
>0.9.13及之后

##  enableVideo(enable)
boolean类型参数
是否开启视频流处理
>0.9.13及之后

## skipErrorFrame()
跳过错误帧,尽可能的不显示因传输丢包造成的花屏\灰色\拖拉\半截不全的画面.
只在软解时生效,跳过时画面会暂停.
播放http-flv基于TCP,掉包会重传,只是延迟增加卡顿明显.
 出现上述画面多为推流端掉包,如udp-rtsp,udp-webrtc, 28181等
当无法改善推流状况时,在播放端开启该方法能提升播体验.
>0.12.4及之后

## release(loseContext)
loseContext 默认为空，false

释放底层WebGL，WebAudio，Socket等资源
当loseContext为true时，会完全销毁canvas上的WebGL Context
>v0.5.41及以后版本
> 注意，除非canvas是通过document.createElement("canvas") 创建，否则不要使用true


## 事件
通过 player.on('事件',事件处理器) 处理

### 'start'
当网络连接成功并有数据返回时回调

### ‘stop’
当本地调用stop方法或远端断开连接时回调

### 'error'
当连接错误或播放中发生错误时回调，1个回调参数
e: 错误信息

### 'audioInfo'
当解析出音频信息时回调，3个回调参数
r：采样率
c：声道数
codec : 编码名

### 'videoInfo'
当解析出视频信息时回调，3个回调参数
w：视频宽
h：视频高
codec : 编码名

### 'videoFrame'
当前渲染的视频pts，1个回调参数
pts ：单位毫秒

### 'videoSei' 
当前解析出h264 sei信息时回调，2个回调参数
sei : Uint8Array格式
pts : 单位毫秒
>v0.5.32及以后版本

### 'stats'
流状态统计，流开始播放后回调，每秒1次，1个回调参数
s:
{
 buf: 当前缓冲区时长，单位毫秒,
 fps: 当前视频帧率,
 abps: 当前音频码率，单位bit,
 vbps: 当前视频码率，单位bit，
 ts:当前视频帧pts，单位毫秒
}

### 'buffer'
缓冲状态回调，1个回调参数
state:状态
有三种状态 empty, buffering, full
>v0.5.41及以后版本

### 'timeout'
当设定的超时时间内无数据返回,则回调
>v0.5.47及以后版本

### 'metadata'
当flv流中包含metadata数据，则回调，可以用内置AMF进行解析
>v0.5.60及之后版本

### 'audioState'
当start()或audioResume()进行音频恢复时回调, (isRunning:boolean, state:string)两个参数
