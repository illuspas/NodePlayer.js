## NodePlayer有多种接入方法，所有接入方法都必须要配合Canvas使用并且需要定义id
```html
<div>
	<canvas id="video1" style="width:640px;height:480px;background-color: black;"></canvas>
</div>
```

### 传统开发方式，在html中引入播放器

```js
 <script type="text/javascript" src="./NodePlayer.min.js"></script>
```

### asmjs 创建播放器对象
```js
 var player = new NodePlayer();
```

### wasm 创建播放器对象
```js
 var player;
 NodePlayer.load(()=>{
 	player = new NodePlayer();
 });
```
>WASM版由于chrome下必须异步编译，因此和asm版引入js库后直接new有区别，必须等待wasm编译完成才能使用

```js
await NodePlayer.asyncLoad()
var player = new NodePlayer();
```
>如果你采用async await 方式编写代码可用该方法

### esm 版
1.1版本后提供esm版，该版本可以在react,vue等开发中直接import 导入，并进行混合编译，自动拷贝wasm文件。无需再修改index.html文件。

#### vue的导入法
```js
<script setup>
import { ref, onMounted, nextTick, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';
import NodePlayerFactory from './NodePlayer-esm.min.js';
// 播放器实例
const playerRef = ref(null);

onMounted(async () => {
  // 初始化 NodePlayer
  const { NodePlayer } = await NodePlayerFactory();
  const player = new NodePlayer();
  player.on("stats",(stats) => { 
    console.log("player stats",stats);
  })
  player.on("error",(error) => { 
    console.log("player error",error);
  })
  player.setView("videoView");
  player.setBufferTime(1000);
  player.start("https://live.nodemedia.cn:8443/live/bbb_264.flv");
  playerRef.value = player;
```

#### react的导入法
```js
import { useState, useEffect, useRef } from 'react';
import { useParams, useNavigate } from 'react-router-dom';
import './LiveDetail.css';
import NodePlayerFactory from "./NodePlayer-esm.min";
const { NodePlayer } = await NodePlayerFactory();

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

### 绑定视图
```js
 player.setView("video1");
```
### 监听播放器状态
```
player.on("start", () => {
// 当连接成功并收到数据
});

player.on("stop", () => {
// 当本地stop或远端断开连接
});

player.on("error", (e) => {
// 当连接错误或播放中发生错误
});

player.on("videoInfo", (w, h) => {
	//当解析出视频信息时回调
	console.log("player on video info width=" + w + " height=" + h);
})

player.on("audioInfo", (r, c) => {
	//当解析出音频信息时回调
	console.log("player on audio info samplerate=" + r + " channels=" + c);
})

player.on("stats", (stats) => {
// 每秒回调一次流统计信息
	console.log("player on stats=", stats);
})
```
> 必须监听error事件

### 开始播放
```js
player.start("http://pull.yourdomain.com/live/stream.flv");
```

### 停止播放
```js
player.stop();
```

## 二、&lt;node-player&gt;标签模式
v1.0.0开始支持的模式，无需js代码，只需html标签&lt;node-player&gt;，与&lt;video&gt;标签用法一致
### 引入播放器js
```
 <script type="text/javascript" src="./NodePlayer.min.js"></script>
```
### 一行html代码实现自动播放
```
<node-player slot="media" scaleMode="1" bufferTime="500" stats streamType="live" muted autoplay
src="http://192.168.0.2:8000/live/bbb.flv "></node-player>
```
