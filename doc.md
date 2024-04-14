<div align="center">
  <p><a href="https://handsfree.js.org"><img src="https://media2.giphy.com/media/BBcnSU1IJ5tpQbwXDI/giphy.gif" alt="handsfree.js.org" title="handsfree.js.org"></a></p>
  <p>快速将面部、手部和/或姿态跟踪集成到你的前端项目中 ✨👌</p>
  <p>
    <img class="mr-1" src="https://img.shields.io/github/tag/handsfreejs/handsfree.svg"> <img class="mr-1" src="https://img.shields.io/github/last-commit/handsfreejs/handsfree.svg">
    <img src="https://img.shields.io/github/repo-size/handsfreejs/handsfree.svg">
  </p>
  <p>
    <img class="mr-1" src="https://img.shields.io/github/issues-raw/handsfreejs/handsfree.svg"> <img src="https://img.shields.io/github/issues-pr-raw/handsfreejs/handsfree.svg">
  </p>
  <p>由以下技术驱动：</p>
  <p>
    <a href="https://mediapipe.dev"><img src="https://i.imgur.com/VGSWYrC.png" height=30></a>
    &nbsp;&nbsp;&nbsp;
    <a href="https://github.com/tensorflow/tfjs-models/"><img src='https://i.imgur.com/Z5PUig3.png' height=30></a>
    &nbsp;&nbsp;&nbsp; 
    <a href="https://github.com/jeeliz/jeelizWeboji"><img width=100 src="https://jeeliz.com/wp-content/uploads/2018/01/LOGO_JEELIZ_BLUE.png"></a>
</div>

<br>
<br>
<br>
<hr>
<br>
<br>
<br>

<h1 style="color: red">💻 项目文档</h1>
我仍在尝试各种方式创建文档。你可以在以下地方找到文档：

- https://handsfree.js.org - 这是本地运行的文档，也是最早的文档
- https://handsfree.dev - 这是托管在 WordPress 上的新网站，包括 Handsfree 插件库的起始部分
- https://codemedium.com/b1f09b783c034644acc1c873f347d6da - 这是 Notion 版本的文档

对于可能存在的混淆，我感到抱歉！我可能会最终选择 Notion，但我仍在寻找最佳的文档。谢谢！

<br>
<br>
<br>
<hr>
<br>
<br>
<br>

# 目录

此存储库主要分为三个部分：实际的库本身位于 `/src/`，其文档位于 `/docs/`，而 Handsfree 浏览器扩展位于 `/extension/`。

- Handsfree.js
  - [通过 CDN 快速开始](#installing-from-cdn) (开始最快的方式)
  - [通过 NPM 快速开始](#installing-from-npm) (需要服务器或打包器)
- [开发指南](#local-development)
- [Handsfree 浏览器扩展](#the-handsfree-browser-extension)

<br>
<br>
<br>
<hr>
<br>
<br>
<br>

# 快速开始

## 从 CDN 安装

> 注意：从 CDN 加载的模型在初始页面加载时可能会较慢，但一旦被浏览器缓存，加载速度应该会快得多。

如果你没有或不需要服务器，或者你在 [像 CodePen 这样的网站](https://codepen.io/MIDIBlocks/pen/mdrwYzM)上进行原型制作，这个选项非常好。你也可以[直接下载这个仓库](https://github.com/MIDIBlocks/handsfree/archive/master.zip)并使用其中的 `/boilerplate/`。

```html
<head>
  <!-- 包含 Handsfree.js -->
  <link
    rel="stylesheet"
    href="https://unpkg.com/handsfree@8.5.1/build/lib/assets/handsfree.css"
  />
  <script src="https://unpkg.com/handsfree@8.5.1/build/lib/handsfree.js"></script>
</head>

<body>
  <!-- 你的代码必须在 body 内部，因为它会对其应用类 -->
  <script>
    // 让我们使用手部追踪，并显示带有线框的网络摄像头
    const handsfree = new Handsfree({ showDebug: true, hands: true });
    handsfree.start();

    // 创建一个名为 "logger" 的插件，以在每一帧上显示数据
    handsfree.use("logger", (data) => {
      console.log(data.hands);
    });
  </script>
</body>
```

## 从 NPM 安装

```bash
# 从你的项目根目录开始
npm i handsfree
```

```js
// 在你的应用内部
import Handsfree from "handsfree";

// 让我们使用手部追踪并启用标记为 "browser" 的插件
const handsfree = new Handsfree({ showDebug: true, hands: true });
handsfree.enablePlugins("browser");
handsfree.start();
```

## 托管模型

上述方法将从 [Unpkg CDN](https://unpkg.com/browse/handsfree@8.5.1/build/lib/assets) 加载模型，其中一些模型超过 10Mb。如果你更愿意自己托管这些模型（例如，离线使用），那么你可以将 npm 包中的模型弹出到你项目的公共文件夹：

```bash
# 将模型移动到你项目的公共目录
# - 将下面的 PUBLIC 改为你存放项目资源的地方

# 在 Windows 上
xcopy /e node_modules\handsfree\build\lib PUBLIC
# 在其他地方
cp -r node_modules/handsfree/build/lib/* PUBLIC
```

```js
import Handsfree from "handsfree";

const handsfree = new Handsfree({
  hands: true,
  // 将此设置为你将模型移入的地方
  assetsPath: "/PUBLIC/assets",
});
handsfree.enablePlugins("browser");
handsfree.start();
```

<br>
<br>
<br>
<hr>
<br>
<br>
<br>

# 示例工作流

以下旨在给你一个如何工作的快速概览。关键要点是一切都围绕 hooks/plugins，它们基本上是在每一帧上运行的命名回调，可以开启和关闭。

## 快速开始工作流

以下工作流演示了如何使用 Handsfree.js 的所有功能。查看 [指南](/guides/) 和 [参考](/ref/) 以深入了解，如果你遇到困难，可以在 [Google Groups](https://groups.google.com/g/handsfreejs) 或 [Discord](https://discord.gg/JeevWjTEdu) 上发帖！

```js
// 让我们启用默认的 Face Pointer 进行面部跟踪
const handsfree = new Handsfree({weboji: true})
handsfree.enablePlugins('browser')

// 现在让我们开始
handsfree.start()

// 创建一个名为 "logger" 的插件
// - 插件在每一帧上运行，这就是你如何“插入”主循环
// - "this" 上下文是插件本身。在这种情况下，是 handsfree.plugin.logger
handsfree.use('logger', data => {
  console.log(data.weboji.morphs, data.weboji.rotation, data.weboji.pointer, data, this)
})

// 让我们现在切换到手部跟踪。为了演示你可以实时进行，让我们创建一个插件，当两个眉毛都上升时切换到手部跟踪
handsfree.use('handTrackingSwitcher', {weboji} => {
  if (weboji.state.browsUp) {
    // 禁用此插件
    // 与 handsfree.plugin.handTrackingSwitcher.disable() 相同
    this.disable()

    // 关闭面部跟踪并启用手部跟踪
    handsfree.update({
      weboji: false,
      hands: true
    })
  }
})

// 你可以启用和禁用任何组合的模型和插件
handsfree.update({
  // 禁用当前正在运行的 weboji
  weboji: false,
  // 启动姿态模型
  pose: true,

  // 这也是你一次配置（或预配置）一堆插件的方式
  plugin: {
    fingerPointer: {enabled: false},
    faceScroll: {
      vertScroll: {
        scrollSpeed: 0.01
      }
    }
  }
})

// 禁用所有插件
handsfree.disablePlugins()
// 仅启用用于制作音乐的插件（尚未实现）
handsfree.enablePlugins('music')

// 重写我们的 logger 以显示原始模型 API
handsfree.plugin.logger.onFrame = (data) => {
  console.log(handsfree.model.pose?.api, handsfree.model.weboji?.api, handsfree.model.pose?.api)
}
```

<br>
<br>
<br>
<hr>
<br>
<br>
<br>

# 示例

## 面部跟踪示例

<table>
  <tr>
    <td>
      <p><strong>面部指针</strong></p>
      <p><img src="https://media4.giphy.com/media/Iv2aSMS0QTy2P5JNCX/giphy.gif"></p>
    </td>
    <td>
      <p><strong>运动视差显示</strong></p>
      <p><img src="https://media4.giphy.com/media/8sCpFH9JCws8iWsaoj/giphy.gif"></p>
    </td>
  </tr>
  <tr>
    <td>
      <p><strong>操纵工业机器人</strong></p>
      <p><img src="https://media4.giphy.com/media/1XE2rnMPk6BFu8VQRr/giphy.gif"></p>
    </td>
    <td>
      <p><strong>使用面部点击玩桌面游戏</strong></p>
      <p><img src="https://media4.giphy.com/media/YATR9GZSSHKeNw3fht/giphy.gif"></p>
    </td>
  </tr>
</table>

<hr>

## 手部跟踪示例

<table>
  <tr>
    <td>
      <p><strong>手部指针</strong></p>
      <p><img src="https://media4.giphy.com/media/FxLUuTSxXjJPx8K9L4/giphy.gif"></p>
    </td>
    <td>
      <p><strong>用于 Three.js 的使用</strong></p>
      <p><img src="https://media4.giphy.com/media/brC1Ow2v62htVmpfLh/giphy.gif"></p>
    </td>
  </tr>
  <tr>
    <td>
      <p><strong>用捏合点击玩桌面游戏</strong></p>
      <p><img src="https://media4.giphy.com/media/pdDOkUpnRbzMk8r0L4/giphy.gif"></p>
    </td>
    <td>
      <p><strong>用你的手指做激光指针</strong></p>
      <p><img src="https://media4.giphy.com/media/2vcbWI2ZAPeGvJVpII/giphy.gif"></p>
    </td>
  </tr>
</table>

---

## 姿态估计示例

<table>
  <tr>
    <td width="50%">
      <p><strong>Flappy Pose - 你需要挥动手臂的 Flappy Bird
