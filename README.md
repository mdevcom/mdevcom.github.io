![Icon](./itd.svg)
# DevCom
一个可以共同协作的平台。
在一个平台里，创作、分享、聊天。

## 前言

DevCom 简称DC，是Mbilse LLC与DevCom Platform Organization(DCPO)开发的基于Google Firebase数据库及Cloudinary云平台的聊天式开发者互动平台，采用Apache License 2.0 协议制作。

## 核心亮点：

- 1. 《美观》的UI
- 2. 快速的聊天服务
- 3. 文件快传服务
- 4. 《神秘》的Kida(基达)动态文件数据流机制
- 5. 原生的Markdown渲染支持
- 6. 原生的语法高亮优化
- 7. 优雅的评论菜单
- 8. CreatingEvent记录单(灰度测试)

## 任务列表

- [x] 美丽的Index Page
- [x] 用户注册
- [x] 用户登录
- [x] Dashboard页面
- [x] 暗色模式
- [x] 聊天(firebase)
- [x] 发文件(cloudinary)
- [ ] 保留/全局更新
- [x] Kida机制
- [x] 下载权控制
- [x] ReKida机制
- [x] 多文件的动态Kida
- [x] Kida Action流
- [x] 发帖子
- [x] 发帖子的评论
- [x] 发帖子的评论的回复
- [x] 删除评论
- [x] 删除帖子
- [x] 删除回复
- [x] Markdown渲染
- [x] 用户中心
- [x] 点赞帖子，评论，回复
- [x] CreatingEvent机制
- [ ] 桌面发行版
- [ ] Clockinary 流采集技术
- [x] 用户点评
- [x] 用户设置
- [ ] 智能登录注册验证
- [x] 关注
- [x] 群聊
- [x] 单聊
- [x] 撤回消息
- [x] 图标包改为Font Awesome 
- [x] 群设置
- [x] 转发消息
- [x] Profile 编辑

## 本地部署：

本地部署外，你也可以用[官网](https://mdevcom.github.io/) 的默认库，登录后直接能用。

推荐全局安装 docsify-cli 工具。

```bash

npm i docsify-cli -g

```

克隆仓库：

```bash

git clone https://github.com/mdevcom/mdevcom.github.io.git

```

运行：

```bash

docsify serve mdevcom.github.io

```

默认用的还是系统服务器，可以尝试使用[Firebase](https://console.firebase.google.com)新建项目：

- 点击 创建项目
- 输入项目名（比如：devcom）
- 创建项目
- 点击网页图标 </>
- 填一个应用名（比如 devcom）
- 点注册
- 保存类似这样的代码，替换到文件：
``` javascript

const firebaseConfig = {
  apiKey: "...",
  authDomain: "...",
  projectId: "...",
  ...
};

```
- 启用"firestore"，"邮箱验证"
Firestore:
- 点击“创建数据库”
- 选择 测试模式（先方便调试）
- 选默认地区
- 修改规则：
```javascript

rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 用户只能读写自己的资料
    match /users/{userId} {
      allow read: if true;   // 公开资料允许所有人查看
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    // 已登录用户可访问动态流
    match /feed/{docId} {
      allow read: if request.auth != null;
      allow create: if request.auth != null;
      allow update: if request.auth != null && request.auth.uid == resource.data.userId;
      allow delete: if request.auth != null && request.auth.uid == resource.data.userId;
    }
  }
}

```
接下来，注册Cloudinary
1. 登录后，进入 Settings → Upload → Upload presets，点击 “Add Upload Preset”。
2. 将 Signing Mode 设置为 Unsigned，并填写一个你记得住的 Preset name，之后在代码中会用到。
3. 保存预设。

在你的 dashboard.html 中，找到 <script type="module"> 的起始位置，紧接着替换下面的上传函数。请务必把 YOUR_CLOUD_NAME 和 YOUR_UPLOAD_PRESET 换成你自己的信息。

```javascript
// ==================== Cloudinary 配置 ====================
const CLOUDINARY_CLOUD_NAME = 'YOUR_CLOUD_NAME';      // 你的 Cloud Name
const UPLOAD_PRESET = 'YOUR_UPLOAD_PRESET';           // 刚刚创建的 Upload Preset

```

就完成了。
