# Mindustry Web · 像素工厂网页版

基于 [Mindustry Classic](https://github.com/Anuken/Mindustry-Classic) 克隆改造的网页版本。

## 🎮 快速开始

本地启动 HTTP 服务器：

```bash
cd /workspace/mindustry-web
python3 -m http.server 8080 --bind 0.0.0.0
```

然后浏览器打开：
- 启动页：http://localhost:8080/launcher.html
- 游戏页：http://localhost:8080/index.html

## 🎯 操作说明

| 按键 | 功能 |
|------|------|
| 左键 | 放置建筑 |
| 右键 | 拆除建筑 |
| WASD / 方向键 | 移动视角 |
| 鼠标滚轮 | 缩放 |
| F | 全屏 |
| ESC | 暂停 |

## 📁 项目结构

```
/workspace/
├── Mindustry-Classic/          # 克隆的源码仓库
│   ├── core/                   # 核心游戏逻辑
│   ├── html/                   # HTML5 (GWT) 模块
│   └── ...
├── mindustry-web/              # 构建产物（可直接部署）
│   ├── launcher.html           # ✨ 自定义启动页（改造新增）
│   ├── index.html              # 游戏入口页（已改造样式）
│   ├── styles.css              # 自定义 CSS（已改造）
│   ├── html/                   # GWT 编译的 JS
│   └── assets/                 # 游戏资源
└── README.md
```

## 🔧 构建过程中修复的问题

1. **jcenter 关闭** → 替换为阿里云镜像仓库
2. **Android SDK 缺失** → 从 settings.gradle 移除 android/ios 模块
3. **网络超时** → 配置 Gradle 使用代理 (127.0.0.1:18080)
4. **Gradle wrapper 无法下载** → 手动下载 gradle-4.10.2 并解压

## 🛠️ 构建

如需从源码重新构建 HTML5 版本：

```bash
# 1. 下载 gradle-4.10.2（项目用的版本）
curl -x http://127.0.0.1:18080 -L -o /tmp/gradle.zip https://services.gradle.org/distributions/gradle-4.10.2-bin.zip
unzip -q /tmp/gradle.zip -d /tmp/

# 2. 使用 Java 8
export JAVA_HOME=/root/.local/share/mise/installs/java/temurin-8.0.504+1
export PATH=$JAVA_HOME/bin:/tmp/gradle-4.10.2/bin:$PATH

# 3. 配置 Gradle 代理
mkdir -p ~/.gradle
cat > ~/.gradle/gradle.properties << 'EOF'
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=18080
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=18080
EOF

# 4. 构建
cd /workspace/Mindustry-Classic
gradle :html:dist --no-daemon
# 产物在 html/build/dist/
```

## 📜 许可证

GPL-3.0（与原项目一致）
