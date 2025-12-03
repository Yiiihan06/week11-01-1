# 视频文件设置指南

## 📹 需要的视频文件

HUD 启动页面需要一个 **hands scanning** 视频文件来显示手部扫描动画。

### 文件要求

**文件名：** `hands scanning.mov`

**存放位置：** 与 `hud-boot-screen.html` 同一目录

**支持的格式：**
- `.mov` (QuickTime) - 推荐
- `.mp4` (H.264) - 备选格式
- `.webm` (VP9) - 网页优化格式

**建议规格：**
- 分辨率：1920x1080 或 1280x720
- 时长：2-4秒（会循环播放）
- 帧率：30fps 或 60fps
- 编码：H.264 或 H.265
- 背景：透明或黑色背景（可选）

## 📁 文件结构

```
week11-01-1/
├── hud-boot-screen.html          # 主启动页面
├── hud-boot-screen-demo.html     # 演示版本
├── hands scanning.mov             # ← 视频文件放这里
├── HUD-README.md                  # 使用文档
└── VIDEO-SETUP.md                 # 本文件
```

## 🎬 视频内容建议

视频应该展示：
1. **手部识别动画** - 扫描框、网格、或光线效果
2. **科幻风格** - 符合 HUD 绿色暗调主题
3. **循环播放** - 开始和结束帧应该能平滑衔接
4. **手势展示** - （可选）显示张开的手掌轮廓

### 示例视频效果
- 手部轮廓识别动画
- 生物特征扫描线
- 指纹或掌纹识别效果
- 神经网络分析可视化
- Matrix 风格的数据流

## 🔧 格式转换

如果您有其他格式的视频，可以使用以下工具转换：

### 使用 FFmpeg（命令行）

转换为 MP4：
```bash
ffmpeg -i input.mov -c:v libx264 -preset slow -crf 22 "hands scanning.mp4"
```

转换为 WebM：
```bash
ffmpeg -i input.mov -c:v libvpx-vp9 -crf 30 -b:v 0 "hands scanning.webm"
```

调整分辨率：
```bash
ffmpeg -i input.mov -vf scale=1280:720 "hands scanning.mov"
```

### 在线转换工具
- [CloudConvert](https://cloudconvert.com/) - 支持多种格式
- [Online-Convert](https://www.online-convert.com/) - 免费转换
- [Convertio](https://convertio.co/) - 快速转换

## 🚀 测试视频

### 方法 1：使用演示版本
1. 将视频文件放在正确位置
2. 打开 `hud-boot-screen-demo.html`
3. 点击 INITIALIZE 按钮
4. 观察视频是否正常播放

### 方法 2：使用完整版本
1. 启动本地服务器：
   ```bash
   python3 -m http.server 8000
   ```
2. 访问 `http://localhost:8000/hud-boot-screen.html`
3. 允许摄像头权限
4. 对着摄像头张开五指
5. 观察视频是否在屏幕中央播放

## ⚙️ 代码配置

如果您的视频文件名不同，需要修改 `hud-boot-screen.html` 中的代码：

找到这一行（约第447行）：
```html
<video id="hand-scanning-video" muted loop>
    <source src="hands scanning.mov" type="video/mp4">
    <source src="hands scanning.webm" type="video/webm">
</video>
```

修改为您的文件名：
```html
<video id="hand-scanning-video" muted loop>
    <source src="your-video-name.mp4" type="video/mp4">
</video>
```

## 🎨 视频样式自定义

### 调整视频大小

在 CSS 中找到 `#hand-scanning-video`（约第104行）：
```css
#hand-scanning-video {
    width: 600px;        /* 修改这里改变宽度 */
    height: auto;
    max-width: 80vw;     /* 响应式最大宽度 */
    ...
}
```

### 调整边框效果

```css
#hand-scanning-video {
    border: 2px solid #00aa33;                    /* 边框 */
    box-shadow: 0 0 40px rgba(0, 170, 51, 0.6);  /* 发光效果 */
    border-radius: 8px;                           /* 圆角 */
}
```

## ❓ 常见问题

### Q: 视频不显示怎么办？
A:
1. 检查文件名是否完全匹配（包括空格）
2. 确认文件在正确的目录
3. 打开浏览器控制台查看错误信息
4. 尝试使用 MP4 格式

### Q: 视频播放卡顿？
A:
1. 降低视频分辨率（720p 足够）
2. 降低视频码率
3. 使用 H.264 编码（兼容性最好）

### Q: 视频无法循环播放？
A: 确保 HTML 中有 `loop` 属性：
```html
<video id="hand-scanning-video" muted loop>
```

### Q: 可以使用 GIF 动画吗？
A: 不推荐。GIF 文件较大且质量较差。建议使用视频格式。

### Q: 视频有声音怎么办？
A: 确保 HTML 中有 `muted` 属性：
```html
<video id="hand-scanning-video" muted loop>
```

## 📦 示例资源

如果您需要现成的手部扫描动画，可以从以下网站获取：

**免费视频素材：**
- [Pexels Videos](https://www.pexels.com/videos/) - 免费高质量视频
- [Pixabay Videos](https://pixabay.com/videos/) - 免费视频素材
- [Videvo](https://www.videvo.net/) - 免费和付费视频

**搜索关键词：**
- "hand scanning animation"
- "biometric scan effect"
- "fingerprint recognition"
- "HUD interface animation"
- "futuristic hand scan"

**付费素材库：**
- [VideoHive](https://videohive.net/) - 专业 HUD 动画
- [Motion Array](https://motionarray.com/) - 高品质视频模板
- [Storyblocks](https://www.storyblocks.com/) - 订阅制素材库

## 🎯 最佳实践

1. **文件大小** - 保持在 5MB 以下以确保快速加载
2. **视频时长** - 2-3秒循环效果最佳
3. **颜色主题** - 使用绿色或蓝色调匹配 HUD 风格
4. **背景** - 黑色或透明背景效果最好
5. **压缩** - 适当压缩以平衡质量和文件大小

## 📝 技术支持

如果您在设置视频时遇到问题：

1. 检查浏览器控制台的错误消息
2. 确认文件路径和名称正确
3. 验证视频格式和编码
4. 测试不同的浏览器

---

**提示：** 如果您暂时没有合适的视频文件，系统会在视频加载失败时显示提示信息，但其他功能仍然正常工作。
