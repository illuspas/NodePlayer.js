本教程使用Vite+Vue3进行集成，使用到NodePlayer-esm版，请在授权开发包内找到该版本或联系客服更新。本集成方法不同于早期修改index.html的版本，可直接import播放器，且一起编译进项目中，wasm文件自动拷贝。

## 1.向播放器加入到项目目录中
解压开发包，将
	NodePlayer-esm.min.d.ts 
	NodePlayer-esm.min.js 
	NodePlayer-esm.min.wasm 
三个文件放置到组件源码目录内
![](https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=2811ac5b65302fe809aa027817ccd925)

## 2.在模板布局中加入canvas组件, 注意一定要定义id
```
<!-- 视频直播画面 -->
<div class="video-container">
	<canvas id="videoView" class="video-view"></canvas>
</div>
```

## 3.在组件源码中导入播放器工厂并创建播放器实例
```
<script setup>
import { ref, onMounted, nextTick, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';
import NodePlayerFactory from './NodePlayer-esm.min.js'; // 加入这一行


// 播放器实例
const player = ref(null);

```

## 4.在组件加载时创建播放器实例并绑定视图，开始自动播放
```
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
  player.start("http://192.168.0.2:8000/live/bbb.flv");
  playerRef.value = player;
```

## 5.在组件卸载时停止播放并销毁对象
```
onUnmounted(() => {
  playerRef.value.stop();
  playerRef.value.release();
})

```

## 注：如果你的项目使用Suspense
<Suspense> 是一个内置组件，用来在组件树中协调对异步依赖的处理。它让我们可以在组件树上层等待下层的多个嵌套异步依赖项解析完成，并可以在等待时渲染一个加载状态。那么你可以在<script setup>后直接拿到NodePlayer

```
<script setup>
import { ref, onMounted, nextTick, onUnmounted } from 'vue';
import { useRouter } from 'vue-router';
import NodePlayerFactory from './NodePlayer-esm.min.js'; // 加入这一行
const { NodePlayer } = await NodePlayerFactory(); //可以顶层await
```


## 项目demo 及源码

<video src="https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=2a44b694675a1727629413ecb8235318" style="width: 320; height: 100%;" controls="controls"></video>

下载demo源码直接体验：[nodeplayer-vue-demo.zip](https://www.nodemedia.cn/doc/server/index.php?s=/api/attachment/visitFile&sign=9266261086ee045bd342ee3c9e1f1c9c "[nodeplayer-vue-demo.zip")
