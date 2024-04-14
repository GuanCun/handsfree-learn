# handsfree-learn

> 当前 handsfree 版本：v8.5.1，它能快速将面部、手部或姿态跟踪集成到你的前端项目中。

## ‼️ 作者的官方示例：

- [handsfreejs v8.5.1](https://handsfreejs.netlify.app/#installing)
- [handsfreejs v6.0.3](https://dev.to/checkboxoz/handsfree-js-a-web-based-face-pointer-24m1)

## 版本差异：

> 在使用上，请注意版本，网上作者的示例版本是 v6.0.3，而当前版本是 v8.5.1。

v8.5.1 版本的 handsfree 有一些新的 API 调整，如：

- 它增加 model 模式，需要一开始就设置到 config 的 Handsfree 实例中。
- 在实践回调函数参数中，包裹多了一层当前模型对象。

## model 模式

- facemesh 面具
- handpose 3D 手型
- hands 2D 手型
- pose 2D 全身姿态
- weboji 表情跟踪

## 备注

- 在 doc 文档可以看到 v8.5.1 版本的所有指南。
- 我把源码保存在本地，是因为作者已经把 github 仓库删除了。
- 现在的源码只在 npm 上：https://www.npmjs.com/package/handsfree?activeTab=code

## 扩展

- [了解下 handsfree.js-集成手势面部表情的前端库](https://juejin.cn/post/7319674466169159715)
- [Handtrack.js](https://victordibia.com/handtrack.js/#/) 是一个轻量级的手部跟踪库，它可以在浏览器中实时检测手部。
- [handsfree 作者 v6.0.0 版本的示例](https://dev.to/checkboxoz/handsfree-js-a-web-based-face-pointer-24m1)
