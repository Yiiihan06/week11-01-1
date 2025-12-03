# Head Up Protocol - HUD Boot Screen

一个科幻风格的全屏启动页面，具有摄像头手势检测和 Matrix 绿色 HUD 主题。

## 功能特性

### 🎨 视觉效果
- **黑色背景** + **暗绿色 HUD UI**（Matrix Green #00ff55）
- 扫描线效果和晕影效果
- 网格背景
- 脉冲和发光动画
- 平滑的淡入淡出过渡

### 🖐️ 手势交互
- 自动启用摄像头
- 检测五指张开手势
- 实时手势识别（基于 MediaPipe Hands）
- 手部扫描动画效果

### 🎯 启动序列
1. **手势检测**：五指张开触发启动
2. **扫描动画**：屏幕中央显示手部扫描框
3. **按钮填充**：INITIALIZE 按钮从左到右填充（3秒）
4. **日志显示**：逐条显示加载日志：
   - ACCESSING OPTICAL SENSORS...
   - BOOTING NEURAL FRAMEWORK...
   - LOADING SATELLITE FEED...
   - INITIALIZING VOICE INTERFACE...
   - SYSTEM READY
5. **淡出**：完成后淡出到主界面

### 💻 技术实现
- 纯 HTML + CSS + JavaScript
- MediaPipe Hands 用于手势检测
- 响应式设计
- 无需外部依赖（除 MediaPipe CDN）

## 使用方法

### 基本使用

1. **在浏览器中打开**：
   ```bash
   # 使用本地服务器（推荐）
   python3 -m http.server 8000
   # 然后访问 http://localhost:8000/hud-boot-screen.html
   ```

2. **允许摄像头权限**：
   - 浏览器会请求摄像头权限
   - 点击"允许"以启用手势检测

3. **触发启动**：
   - 将手掌对准摄像头
   - 张开五指
   - 系统自动开始启动序列

### 后备模式

如果摄像头不可用，按钮会自动启用点击功能：
- 直接点击 **INITIALIZE** 按钮
- 启动序列照常进行

## 集成到项目

### 监听系统启动事件

```javascript
// 监听 HUD 系统启动完成事件
window.addEventListener('hudSystemBooted', (event) => {
    console.log('HUD System Booted:', event.detail);
    // 在这里加载你的主应用
    loadMainApplication();
});
```

### 使用 systemBoot 回调

```javascript
// 重写 systemBoot 函数
window.HUD_Protocol.systemBoot = function() {
    console.log('Custom boot logic');
    // 你的自定义启动逻辑
    initializeMainInterface();
    loadUserData();
    connectToServer();
};
```

### 检查启动状态

```javascript
// 检查系统是否已启动
if (window.HUD_Protocol.isBooted()) {
    console.log('System is ready');
}
```

## 自定义配置

在 HTML 文件中修改 `SYSTEM_CONFIG` 对象：

```javascript
const SYSTEM_CONFIG = {
    scanDuration: 3000,        // 扫描动画时长（毫秒）
    logDelay: 600,             // 日志间隔时间（毫秒）
    fadeOutDuration: 1000,     // 淡出时长（毫秒）
    logs: [                    // 自定义日志消息
        'ACCESSING OPTICAL SENSORS...',
        'BOOTING NEURAL FRAMEWORK...',
        // 添加更多日志...
    ]
};
```

## 主题色自定义

修改 CSS 中的主色调（默认 #00ff55）：

```css
/* 在 <style> 标签中全局替换 */
#00ff55  /* 主绿色 */
#001a0d  /* 暗绿色背景渐变 */
#ff0055  /* 待机状态红色（摄像头指示器）*/
```

## 浏览器兼容性

- ✅ Chrome/Edge 90+
- ✅ Firefox 88+
- ✅ Safari 14+（需要 HTTPS）
- ⚠️ 移动浏览器（需要后置摄像头）

**注意**：摄像头功能需要：
- HTTPS 连接（或 localhost）
- 用户授权摄像头权限
- WebRTC 支持

## 故障排查

### 摄像头无法启动
- 检查浏览器权限设置
- 确保使用 HTTPS 或 localhost
- 尝试刷新页面重新授权

### 手势检测不灵敏
- 确保手掌完全在摄像头视野内
- 保持手掌与摄像头距离适中（30-50cm）
- 在光线充足的环境下使用
- 五指完全张开

### 页面加载慢
- MediaPipe 库需要从 CDN 加载
- 首次加载可能需要几秒钟
- 考虑本地托管 MediaPipe 库

## API 参考

### Window.HUD_Protocol

```javascript
{
    // 系统启动回调函数
    systemBoot: Function,

    // 检查系统是否已启动
    isBooted: () => Boolean
}
```

### 自定义事件

#### hudSystemBooted
系统启动完成后触发

```javascript
event.detail = {
    timestamp: Number,  // 启动时间戳
    status: String      // 状态：'operational'
}
```

## 文件结构

```
hud-boot-screen.html     # 完整的独立 HTML 文件
├── CSS（内嵌）          # 所有样式
├── JavaScript（内嵌）   # 所有逻辑
└── MediaPipe（CDN）     # 外部依赖
```

## 性能优化建议

1. **本地托管 MediaPipe**：
   ```bash
   npm install @mediapipe/hands
   ```

2. **减少动画复杂度**：
   - 在低端设备上禁用某些效果
   - 使用 `will-change` CSS 属性

3. **预加载资源**：
   ```html
   <link rel="preconnect" href="https://cdn.jsdelivr.net">
   ```

## 许可

MIT License

## 技术支持

如有问题或建议，请提交 Issue。

---

**Powered by Head Up Protocol v2.4.7**
*Neural Interface Technology*
