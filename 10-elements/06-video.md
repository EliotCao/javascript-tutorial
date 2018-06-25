# <video>，<audio>

## 概述

```
<video>元素用来加载视频，是HTMLVideoElement对象的实例。<audio>元素用来加载音频，是HTMLAudioElement对象的实例。而HTMLVideoElement和HTMLAudioElement都继承了HTMLMediaElement，所以这两个 HTML 元素有许多共同的属性和方法，可以放在一起介绍。
```

理论上，这两个 HTML 元素直接用`src`属性指定媒体文件，就可以使用了。

```
<audio src="background_music.mp3"/>
<video src="news.mov" width=320 height=240/>
```

注意，`<video>`元素有`width`属性和`height`属性，可以指定宽和高。`<audio>`元素没有这两个属性，因为它的播放器外形是浏览器给定的，不能指定。