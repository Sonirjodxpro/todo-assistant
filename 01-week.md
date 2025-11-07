---
highlight: a11y-dark
---
### 一、环境搭建

#### 基础必要环境

> 仅以mac环境作为演示

##### 1.nodejs 

安装 [nodejs安装指南](https://nodejs.org/zh-cn/download)

```bash
node -v

v22.17.0
```

##### 2.rust

安装 [rust安装指南](https://rust-lang.org/zh-CN/tools/install/)

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

验证
```bash
rustc --version

rustc 1.91.0 (f8297e351 2025-10-28)
```

##### 3.Xcode

应用商店安装即可


##### 4.webView2

这个比较特殊，mac 不需要安装，但window 平台需注意，我查了下自带webView2的windows系统版本

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/31fb5433441843a49e6ec123f73e9377~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oe15oeC55qE6JGx5aS0:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMTQ5MTg5MzEzNDM4ODA4In0%3D&rk3s=e9ecf3d6&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1762587305&x-orig-sign=urulOe08RIaySBbtfH2qZIQUOvE%3D)

##### 5. 安卓和ios 的暂不考虑，主要是以桌面端为准


好的，到这里，安装基础依赖结束，让我们准备迎接 Hello World!

### 二、创建项目

我这里选择的是使用`cargo` (rust 包管理工具)

```bash
cargo install create-tauri-app --locked
cargo create-tauri-app
```

如果使用npm 更简单一些
```bash
npm create tauri-app@latest
```

![微信图片_20251107160031_34.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/2dfdc9f06f9e413aad7182a872a85f19~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oe15oeC55qE6JGx5aS0:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMTQ5MTg5MzEzNDM4ODA4In0%3D&rk3s=e9ecf3d6&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1762588857&x-orig-sign=uiLGBmw6Sz2fXnqkWi%2B2TOz3p8o%3D)


进入工程后

项目结构主要是`src` `src-tauri` 


然后通过 `npm` 命令启动项目 

```bash
npm run tauri dev
```

Hello,World! 成功啦！   ☀️

![image.png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/50a2188913434fa2a4d1d45786a3bd24~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oe15oeC55qE6JGx5aS0:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMTQ5MTg5MzEzNDM4ODA4In0%3D&rk3s=e9ecf3d6&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1762589269&x-orig-sign=MncIHibqIO5ia5Qoz8LN8qAoP8U%3D)


### 三、Tarui 架构个人理解

> Tauri = 用 Web 技术写前端界面 + 用 Rust 做系统层后端，通过 IPC 通信连接两者。

![ChatGPT Image 2025年11月7日 16_21_02 (1).png](https://p0-xtjj-private.juejin.cn/tos-cn-i-73owjymdk6/16a4e3da38cd40389554b6f18287def3~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oe15oeC55qE6JGx5aS0:q75.awebp?policy=eyJ2bSI6MywidWlkIjoiMTQ5MTg5MzEzNDM4ODA4In0%3D&rk3s=e9ecf3d6&x-orig-authkey=f32326d3454f2ac7e96d3d06cdbb035152127018&x-orig-expires=1762590135&x-orig-sign=Zi3wo9A%2B5RutO3SDmkCTIkJbpnQ%3D)


官网的架构图，更多的体现的是系统实现层(运行时架构/平台抽象)


```mermaid
flowchart TD
    classDef box fill:#f4f4f4,stroke:#555,stroke-width:1px,rx:6px,ry:6px;
    classDef dark fill:#333,stroke:#000,stroke-width:1px,color:#fff,rx:6px,ry:6px;

    A[WebView]:::dark --> B[Tauri Core<br/>Commands / IPC]:::box
    B --> C1[Tao]:::box
    B --> C2[Wry]:::box
    C1 --> D[OS]:::box
    C2 --> D[OS]:::box

```


### 四、进程间关系
这个与我上面理解的 个人技术架构上基本一致呢

WebView 进程 和 核心进程

一对多的关系
