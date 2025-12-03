# HUD Boot Screen - Head Up Protocol

一个科幻风格的HUD系统启动页面，采用黑色背景和暗绿色Matrix风格界面。

## 功能特性

### 1. 视觉效果
- 🎨 黑色背景 + 暗绿色HUD主题 (#00aa33)
- ✨ 发光效果和扫描线动画
- 🔲 科幻风格的边角框架
- 💚 INITIALIZE按钮带悬停发光效果

### 2. 手势识别
- 📹 实时摄像头检测（右上角预览）
- ✋ 检测五指张开手势
- ⏱️ 需要持续3秒完成认证

### 3. 交互流程

#### 正常流程：
1. 页面加载后自动启动摄像头
2. 五指张开手势触发扫描
3. 屏幕中央显示hands scanning动画
4. INITIALIZE按钮从左往右填充（3秒）
5. 完成后显示系统启动日志：
   - ACCESSING OPTICAL SENSORS...
   - BOOTING NEURAL FRAMEWORK...
   - LOADING SATELLITE FEED...
   - INITIALIZING VOICE INTERFACE...
   - SYNCHRONIZING BIOMETRIC DATA...
   - ESTABLISHING SECURE CONNECTION...
   - SYSTEM READY
6. 淡出启动页面，进入主界面

#### 失败流程：
- 如果手势未持续3秒即中断
- 按钮填充停止并重置
- hands scanning动画消失
- 显示红色警告："IDENTITY VERIFICATION FAILED. PLEASE TRY AGAIN."
- 2秒后警告消失，可重新尝试

### 4. 系统集成
- 提供 `systemBoot()` 函数供外部调用
- 触发自定义事件 `hudSystemBoot`
- 可通过监听事件加载其他模块

## 使用方法

### 直接打开
```bash
# 使用浏览器打开
open hud-boot-screen.html
```

或直接双击 `hud-boot-screen.html` 文件

### 需要HTTPS或本地服务器
由于摄像头权限要求，建议使用本地服务器：

```bash
# Python 3
python -m http.server 8000

# Node.js (http-server)
npx http-server

# PHP
php -S localhost:8000
```

然后访问: http://localhost:8000/hud-boot-screen.html

### 外部模块集成

```javascript
// 方法1: 监听事件
window.addEventListener('hudSystemBoot', (event) => {
    console.log('HUD启动完成:', event.detail.timestamp);
    // 加载你的主应用
    loadMainApplication();
});

// 方法2: 直接覆盖函数
window.systemBoot = function() {
    console.log('自定义启动逻辑');
    // 你的代码
};
```

## 技术栈

- **HTML5** - 结构
- **CSS3** - 动画和样式
- **JavaScript ES6+** - 逻辑控制
- **MediaPipe Hands** - 手势识别
- **WebRTC** - 摄像头访问

## 浏览器兼容性

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ⚠️ 需要支持 WebRTC 和 getUserMedia

## 设计细节

### 颜色方案
- 主色：`#00aa33` (暗绿色)
- 强调色：`#00ff44` (亮绿色)
- 背景：`#000000` (纯黑)
- 错误：`#ff0000` (红色)

### 动画时长
- 手势扫描：3秒
- 按钮填充：3秒（同步）
- 日志显示：每条0.5秒间隔
- 错误提示：2秒后消失
- 淡出效果：1秒

### 手势识别参数
- 最大手数：1只手
- 检测置信度：0.7
- 追踪置信度：0.7
- 需要所有五指伸展

## 自定义修改

### 修改主题色
在CSS中搜索 `#00aa33` 替换为你想要的颜色

### 修改启动日志
编辑 JavaScript 中的 `logMessages` 数组：

```javascript
const logMessages = [
    'YOUR CUSTOM MESSAGE 1...',
    'YOUR CUSTOM MESSAGE 2...',
    // ...
];
```

### 调整扫描时长
修改 `updateScanProgress()` 中的时间判断：

```javascript
if (elapsed >= 3000) { // 改为你想要的毫秒数
    completeScan();
}
```

## 故障排除

### 摄像头无法访问
1. 确保使用HTTPS或localhost
2. 检查浏览器权限设置
3. 允许摄像头访问权限

### 手势识别不准确
1. 确保光线充足
2. 手掌完全在摄像头视野内
3. 五指完全伸展张开
4. 保持手势稳定3秒

### MediaPipe加载失败
检查网络连接，CDN资源需要互联网访问

## License

MIT License

## 作者

Created for Head Up Protocol HUD System
