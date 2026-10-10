# 工作手记 Mac Intel 构建输入

这是版本 1.3.0 的最小构建输入，**不是已经完成的 Mac 安装包**。只有离线应用代码、Mac 打包配置和人工测试图片；没有个人笔记、用户图片或本机数据。

需要 macOS 13 或更高版本的 Intel / x64 构建环境，Node.js 22 或更高版本。根目录开发依赖固定为 Electron 44.7.0 和 electron-builder 26.15.3。

```sh
npm ci
npm run package:mac
npm run smoke:mac
```

打包输出在 release，原生测试报告为 release/mac-native-smoke.json。smoke 加载实际 app.asar，隔离测试数据，并验证进度保存、图文顺序、图片读取、ZIP 恢复、⌘+S 标签和 Mac 菜单。可以传入 --archive 指向重新解压成品中的 app.asar。测试仅清理其拥有的临时目录。签名当前是本地临时签名，未做 Apple 公证。

verify:mac 仅用于干净的 Intel Mac CI 环境，实际启动打包后的 .app，另存 release/mac-launch-check.json。它不删除本机目录，也不会修改系统安全设置。

准备脚本不会上传任何文件，也不会生成锁文件。它复制项目中已审核的手动触发工作流至 .github/workflows/build-mac.yml，并保留既有 package-lock.json 及其他工作流。目录旁的 build-input-audit.json 是每个待上传文件的路径、大小和 SHA-256 清单，放在目录外以避免清单自校验循环。只上传审核后的这个 build-input 目录；不要包含生成的 release、node_modules、.sites-runtime 或任何用户数据。
