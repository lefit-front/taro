---
title: React Native 端开发流程
---

> 本篇主要讲解 Taro React Native 端 环境安装-开发-调试-打包-发布 原理及流程，React Native 开发前注意事项请看 [开发前注意](https://nervjs.github.io/taro/docs/before-dev-remind.html)

## 简介
Taro RN 端的开发基于开源项目 [Expo](https://expo.io/)，类似于 [create-react-native-app](https://github.com/react-community/create-react-native-app)。

### Expo 简介
> Expo is a set of tools, libraries and services which let you build native iOS and Android apps by writing JavaScript.

Expo 是一组工具，库和服务，基于 React Native 可让您通过编写 JavaScript 来构建原生 iOS 和 Android 应用程序。

Expo 应用程序是包含 Expo SDK 的 React Native 应用程序。 SDK 是一个 native-and-JS 库，可以访问设备的系统功能（如相机，联系人，本地存储和其他硬件）。这意味着您不需要使用 Xcode 或 Android Studio，也不需要编写任何本机代码，而且它还使得您的 pure-JS 项目非常便于携带，因为它可以在任何包含 Expo SDK 的本机环境中运行，方便开发及调试。

最后，你可以使用 Expo 托管应用，它可以为您提供推送通知，并且可以构建能部署到应用商店 ipa 包或者 apk 包。

更多资料，可以查看 [Expo 官方文档](https://docs.expo.io/versions/latest/)。

> [Expo 版本清单](https://expo.io/--/api/v2/versions)，这里可以看到每个版本 Expo 对应的版本关系，这很重要。

### 为什么选择 Expo？
从某种程度上而言，目前为止 RN 只是给拥有 Mac 电脑的开发者提供了跨平台开发的能力， 因为现在还不能使用 Windows 创建 iOS 的 RN 应用。还有一个比较普遍的问题是，有一些 iOS 程序员不会配置 Android 的编译环境，而一些 Android 程序员又搞不懂 XCode。而且，Taro 的使用者基本都是前端工程师，面对 iOS 和 Android 原生的库或者文件可能会不知所措。

我们希望 Taro 的使用者，即使完全没有 RN 开发经验，也能够从配环境开始 5 分钟实现 Hello Wolrd 的编写，并且只需要专注于基于 Taro 实现功能，不用再去配置烦人的 iOS、Android 编译环境，还可以用 Windows 开发 iOS 版的 RN 应用。而恰好 Expo 可以完美实现。

本质上，Expo 的移动客户端相当于一个壳，你只需关注 js 层面的开发即可。这点类似于 electron 或者小程序。

## 准备工作

#### iOS 模拟器

通过 Apple App Store 安装 [Xcode](https://itunes.apple.com/app/xcode/id497799835)。这会需要一段时间，去小睡一下。接下来，打开 Xcode，转到 首选项（preferences） 并单击 Components 选项卡，从列表中安装一个模拟器。

首次启动模拟器可能需要手动在模拟器上安装 Expo 客户端，

您可以按照以下步骤操作：

- 下载最新的模拟器构建。
- 提取存档的内容。你应该得到一个像 `Exponent-X.XX.X` 这样的目录。
- 确保模拟器正在运行。
- 在终端上，运行 `xcrun simctl install booted [提取目录的路径]`。

![image](https://user-images.githubusercontent.com/9441951/44649246-e6eb1000-aa15-11e8-849e-f4bc17eeccab.png)

#### Android 模拟器

[下载 Genymotion](https://www.genymotion.com/fun-zone/)（免费版）并按照 [Genymotion 安装指南](https://docs.genymotion.com/Content/01_Get_Started/Installation.htm)。安装 Genymotion 后，创建一个虚拟设备，准备好后启动虚拟设备。

如果遇到任何问题，请按照 Genymotion 指南进行操作。

#### 移动客户端：Expo (适用于 iOS 和 Android)

在模拟器或真机上安装 Expo 客户端。

expo 客户端就像是一个用 expo 建造的应用程序浏览器。当您在项目中启动时，**它会为您生成一个开发地址及对应的二维码，您可以在 iOS 或 Android 上使用 expo 客户端上访问它**，无论是使用真机上还是模拟器，原理和步骤都相同。

[Android Play Store 下载地址 ( 或者直接从各大应用商店搜索 )](https://play.google.com/store/apps/details?id=host.exp.exponent) 

 [iOS App Store 下载地址](https://itunes.com/apps/exponent)

> **版本支持:** Android 4.4 及以上、 iOS 9.0 及以上

更多资料可以查看 [Expo 移动客户端文档](https://docs.expo.io/versions/v29.0.0/workflow/up-and-running)

#### 看守者(Watchman)

如果一些 macOS 用户没有在他们的机器上安装它，会遇到问题，因此我们建议您安装 Watchman。 Watchman 在更改时观察文件和记录，然后触发相应的操作，并由 React Native 在内部使用。[下载并安装 Watchman](https://facebook.github.io/watchman/docs/install.html)。

## 开发

### 编译

RN 编译预览模式:

```shell
# npm script
$ npm run dev:rn
# 仅限全局安装
$ taro build --type rn --watch
# npx 用户也可以使用
$ npx taro build --type rn --watch
```

Taro 将会开始编译文件：

```shell
➜  TodoMVC git:(master) ✗ taro build --type rn --watch
👽 Taro v1.0.0-beta.15

开始编译项目 todo-list
编译  JS        /Users/chengshuai/Taro/TodoMVC/src/app.js
编译  SCSS      /Users/chengshuai/Taro/TodoMVC/src/app.scss
编译  JS        /Users/chengshuai/Taro/TodoMVC/src/actions/index.js
....
生成  app.json  /Users/chengshuai/Taro/TodoMVC/.temp/app.json
生成  package.json  /Users/chengshuai/Taro/TodoMVC/.temp/package.json
拷贝  crna-entry.js  /Users/chengshuai/Taro/TodoMVC/.temp/bin/crna-entry.js
编译  编译完成，花费 780 ms
17:12:59: Starting packager...

初始化完毕，监听文件修改中...
```

生成的应用文件在根目录的 `.rn_temp`目录下，常见的工程目录结构如下：

```
./.rn_temp
├── components
│   ├── Footer
│   ├── TodoItem
│   └── TodoTextInput
├── pages
│   └── index
│       ├── index.js
│       └── index.scss
├── node_modules
├── app.js
├── app_styles.js
├── app.json
├── README.md
├── package.json
└── yarn.lock
```

> **Note:** If you are on MacOS and XDE gets stuck on "Waiting for packager and tunnel to start", you may need to [install watchman on your machine](https://facebook.github.io/watchman/docs/install.html#build-install). The easiest way to do this is with [Homebrew](http://brew.sh/), `brew install watchman`.

### 启动
如果编译过程没有报错，`Packager Started` 后，你将会看到以下内容：

![image](https://user-images.githubusercontent.com/9441951/45069323-89824d80-b0fe-11e8-86ae-2bbe532087de.png)

接下来，你可以直接在终端输入对应的字母，来进行对应的操作：

- a : 打开安卓设备或安卓模拟器
- i  : 打开 iOS 模拟器
- s : 发送 app URL 到手机号或 email 地址
- q : 显示二维码
- r : 重启 packager
- R : 重启 packager 并清空缓存
- d : 开启 development 模式

如果你使用真机，你只需要使用 Expo 应用扫描这个二维码就可以打开你编写的 RN 应用了。并且只要在 Expo 中打开过一次，就会在 App 中保留一个入口。

本质上，Expo 相当于一个壳，你只需关注 js 层面的开发即可。这点类似于 electron 或者小程序。

如果你在终端按下 `i`· ，Taro 将会自动启动 iOS 模拟器，启动 expo 客户端（如果已成功安装），然后加载应用。

![image](https://user-images.githubusercontent.com/497214/28835171-300a12b6-76ed-11e7-81b2-623639c3b8f6.png)

终端将会显示日志：

```shell
17:43:54: Starting iOS...

 › Press a to open Android device or emulator, or i to open iOS emulator.
 › Press s to send the app URL to your phone number or email address
 › Press q to display QR code.
 › Press r to restart packager, or R to restart packager and clear cache.
 › Press d to toggle development mode. (current mode: development)

17:44:05: Finished building JavaScript bundle in 492ms
```



### 开发者菜单

一旦 app 在 expo 中成功打开，你可以通过摇一摇设备来唤起开发者菜单， 如果你是用模拟器，你可以按 `⌘+d` （iOS） 或 `ctrl+m` （Android）。

![image](https://docs.expo.io/static/images/generated/v29.0.0/workflow/developer-menu.png)

更多资料可以查看[Expo 文档——up-and-running](https://docs.expo.io/versions/v29.0.0/workflow/up-and-running)。

## 调试

### 简介

调试方面强烈推荐使用 [React Native Debugger ](https://github.com/jhen0409/react-native-debugger)，一个基于 React Native 官方调试方式、包含 React Inspector / Redux DevTools 独立应用：

- 基于官方的 [Remote Debugger](https://facebook.github.io/react-native/docs/debugging.html#chrome-developer-tools) 且提供了更为丰富的功能
- 包含 [`react-devtools-core`](https://github.com/facebook/react-devtools/tree/master/packages/react-devtools-core) 的 [React Inspector](https://github.com/jhen0409/react-native-debugger/blob/master/docs/react-devtools-integration.md)
- 包含 Redux DevTools, 且与 [`redux-devtools-extension`](https://github.com/zalmoxisus/redux-devtools-extension) 保持 [API](https://github.com/jhen0409/react-native-debugger/blob/master/docs/redux-devtools-integration.md) 一致

![image](https://user-images.githubusercontent.com/3001525/29451479-6621bf1a-83c8-11e7-8ebb-b4e98b1af91c.png)

可以查看文章 [React Native Debugger + Expo = AWESOME](https://medium.com/@jimgbest/react-native-debugger-expo-awesome-d7a00da51460)，了解更多。

### 安装

不同平台及版本的安装包，请点击 [这里](https://github.com/jhen0409/react-native-debugger/releases) 。

**macOS**平台可以使用 [Homebrew Cask](https://caskroom.github.io/) 安装：

```shell
$ brew update && brew cask install react-native-debugger
```

### 启动

在启动 React Native Debugger 之前，请先确认以下内容：

- 所有的 React Native 的 debugger 客户端已关闭，特别是 `http://localhost:<port>/debugger-ui`
- React Native Debugger 会尝试连接 debugger 代理， expo 默认使用 `19001` 端口， 你可以新建一个 debugger 窗口 (macOS: `Command+T`, Linux/Windows: `Ctrl+T`) 开定义端口
- 保证 [developer menu](https://facebook.github.io/react-native/docs/debugging.html#accessing-the-in-app-developer-menu)  的  `Debug JS Remotely` 处于开启状态

你可以启动应用之后再修改端口，也可以直接通过命令行启动应用时指定端口：

```shell
open "rndebugger://set-debugger-loc?host=localhost&port=19001"
```

>  如果启动之后调试窗口空白，请确认调试端口正确。

### 使用 Redux DevTools Extension API

Use the same API as [`redux-devtools-extension`](https://github.com/zalmoxisus/redux-devtools-extension#1-with-redux) is very simple:

```jsx
const store = createStore(
  reducer, /* preloadedState, */
  window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__()
)
```

See [`Redux DevTools Integration`](https://github.com/jhen0409/react-native-debugger/blob/master/docs/redux-devtools-integration.md) section for more information.

### 更多资料

- [快速开始](https://github.com/jhen0409/react-native-debugger/blob/master/docs/getting-started.md)
- [Debugger 整合](https://github.com/jhen0409/react-native-debugger/blob/master/docs/debugger-integration.md)
- [React DevTools 整合](https://github.com/jhen0409/react-native-debugger/blob/master/docs/react-devtools-integration.md)
- [Redux DevTools 整合](https://github.com/jhen0409/react-native-debugger/blob/master/docs/redux-devtools-integration.md)
- [Shortcut references](https://github.com/jhen0409/react-native-debugger/blob/master/docs/shortcut-references.md)
- [Network inspect of Chrome Developer Tools](https://github.com/jhen0409/react-native-debugger/blob/master/docs/network-inspect-of-chrome-devtools.md)
- [Enable open in editor in console](https://github.com/jhen0409/react-native-debugger/blob/master/docs/enable-open-in-editor-in-console.md)
- [Troubleshooting](https://github.com/jhen0409/react-native-debugger/blob/master/docs/troubleshooting.md)
- [Contributing](https://github.com/jhen0409/react-native-debugger/blob/master/docs/contributing.md)

## 打包

如何愉快地打包发布，可能你还在头疼安卓的签名、难缠的 gradle 和各种配置，还在头疼 iOS 打包发布时在 Xcode 来回折腾，为什么不能脱离这些原生开发才需要的步骤呢，ReactNative 本身就是为了统一安卓和 iOS，如今到打包这一步却要区别对待，颇为不妥，expo 就是个很好的解决方案，它提供壳子，我们只需要关心我们自己的代码，然后放进壳里即可。

在打包发布步骤之前，我们先对开发者的源代码进行预处理打包，转成 ReactNative 代码：

``` bash
taro build --type rn
```

得到热腾腾的 React Native 代码，就可以开始进行打包了，打包教程可以查阅 expo 文档：[Building Standalone Apps](https://docs.expo.io/versions/latest/distribution/building-standalone-apps)。

## 发布

### 发布到 expo

expo 的发布教程可以查阅文档：[Publishing](https://docs.expo.io/versions/latest/guides/publishing.html)（发布到 expo 不需要先经过打包），通过 expo 客户端打开发布后的应用 CDN 链接来访问。

![通过 expo 打开一个 app](http://storage.360buyimg.com/temporary/180906-fetch-app-production.png)

发布后的应用有个专属的地址，比如应用 [Expo APIs](https://expo.io/@community/native-component-list)，通过 expo 客户端扫描页面中的二维码进行访问（二维码是个持久化地址 persistent URL）。

### 发布到应用商店

如果你需要正式发布你的独立版应用，可以把打包所得的 ipa 和 apk 发布到 Apple Store 和应用市场，详细参阅 [Distributing Your App](https://docs.expo.io/versions/latest/distribution/index.html)，后续的更新可以通过发布到 expo 更新 CDN 的资源来实现。

## 常见错误

### No bundle url present

导致这个报错的原因很多，最常见的是电脑开了代理。具体可以参考 [#12754](https://github.com/facebook/react-native/issues/12754)

### UnableToResolveError: Unable to resolve module `AccessibilityInfo`

原因很多，我这边是重启电脑就好了😂。 具体可以查看 [#14209](https://github.com/facebook/react-native/issues/14209)

### Metro Bundler error: Expected path […] to be relative to one of the project roots

不支持 `npm link`，可以使用[nicojs/node-install-local](https://github.com/nicojs/node-install-local) 替代。

### Image component does not resolve images with filenames that include '@' symbol
![image](https://user-images.githubusercontent.com/22125059/44312799-373dee80-a3d4-11e8-8367-9cf44e851739.PNG)

React Native 不支持路径中带 @ 符号，具体可以查看 [#14980](https://github.com/facebook/react-native/issues/14980)。

### The development server returned response error code 500
![image](https://user-images.githubusercontent.com/25324938/41452372-42c1e766-708f-11e8-96ce-323eaa1eb03f.jpeg)
多半是依赖的问题，进入 `.rn_temp/`目录，然后删除 npm 依赖，在重新安装就可以了。
也可以试一下以下命令：

```shell
watchman watch-del-all 
rm -rf node_modules && npm install 
rm -fr $TMPDIR/react-*
```

具体可以参考 [#1282](https://github.com/expo/expo/issues/1282)

### Expo client app 加载阻塞： "Building JavaScript bundle... 100%" 
![image](https://user-images.githubusercontent.com/9441951/47762170-7bb00980-dcf6-11e8-95ab-41152076c3de.png)

可能的原因很多，可以参考这个issue：[react-community/create-react-native-app#392](https://github.com/react-community/create-react-native-app/issues/392)

## 参考

-  [expo 官方文档](https://docs.expo.io/versions/latest/)
- [React Native Debugger ](https://github.com/jhen0409/react-native-debugger)
- [Building Standalone Apps](https://docs.expo.io/versions/latest/distribution/building-standalone-apps)
- [Publishing on Expo](https://blog.expo.io/publishing-on-exponent-790493660d24)
