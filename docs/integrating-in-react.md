本教程使用Vite+React进行集成，使用到NodePlayer-esm版，请在授权开发包内找到该版本或联系客服更新。本集成方法不同于早期修改index.html的版本，可直接import播放器，且一起编译进项目中，wasm文件自动拷贝。

## 1.向播放器加入到项目目录中
解压开发包，将
	NodePlayer-esm.min.d.ts 
	NodePlayer-esm.min.js 
	NodePlayer-esm.min.wasm 
三个文件放置到组件源码目录内
![](https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=77ad3fb7282962da53942e4f5a0b25b2)

## 2.在组件中引入播放器
```
import { useState, useEffect, useRef } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import './LiveDetail.css';
import NodePlayerFactory from "./NodePlayer-esm.min";
const { NodePlayer } = await NodePlayerFactory();
```

## 3.在组件布局中加入canvas组件, 注意一定要定义id
```
return (
	<!-- 视频直播画面 -->
	<div class="video-container">
		<canvas id="videoView" class="video-view"></canvas>
	</div>
)
```

## 4.在组件加载后直接开始播放，卸载后停止播放并销毁实例
```
useEffect(() => {

    const player = new NodePlayer();
    player.on("error", (error) => {
      console.log("Player on error", error);
    });
    player.on("stats", (stats) => {
      console.log("Player on stats", stats)
    });
    player.setView("videoView");
    player.setBufferTime(1000);
    player.start("http://192.168.0.2:8000/live/bbb.flv");


    return () => {
      player.stop();
      player.release();
    };

  }, []);
```

## Demo运行效果及源码

<video src="https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=5c3671b51eb8e2f9bb4c7c8f79dd7d5e" style="width: 320px; height: 100%;" controls="controls"></video>

下载demo源码：[nodeplayer-react-demo.zip](https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=407f34be902fb352513b2bb7b54fcbcc "[nodeplayer-react-demo.zip")
