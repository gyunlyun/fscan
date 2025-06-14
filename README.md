# fscan 自动编译发布

这是一个自动编译并发布 [fscan](https://github.com/shadow1ng/fscan) 的 GitHub Actions 项目。当上游 fscan 仓库有新版本发布时，会自动拉取源码并编译出多平台的可执行文件。

## 📋 项目简介

fscan 是一款内网综合扫描工具，支持主机存活探测、端口扫描、常见服务的爆破、ms17010、redis批量写公钥、计划任务反弹shell、读取win网卡信息、web指纹识别、web漏洞扫描、netbios探测、域控识别等功能。

本项目通过 GitHub Actions 自动化流程，实现：
- 🔄 自动检测上游 fscan 仓库的新版本
- 🔨 自动编译多平台可执行文件
- 📦 自动发布到 GitHub Releases
- ⏰ 每日定时检查更新（北京时间下午 4 点）

## 🚀 支持平台

本项目编译以下平台的可执行文件：

| 操作系统 | 架构 | UPX 压缩 | 文件名 |
|---------|------|----------|--------|
| Linux | AMD64 | ✅ | `fscan_linux_amd64` |
| Linux | ARM64 | ✅ | `fscan_linux_arm64` |
| Windows | AMD64 | ✅ | `fscan_windows_amd64.exe` |
| Windows | ARM64 | ❌ | `fscan_windows_arm64.exe` |
| macOS | AMD64 | ❌ | `fscan_darwin_amd64` |
| macOS | ARM64 | ❌ | `fscan_darwin_arm64` |

> **注意**：由于 UPX 不支持 Windows ARM64 和 macOS 平台，这些版本不会进行压缩。

## 📥 下载使用

1. 访问本项目的 [Releases 页面](../../releases)
2. 选择最新版本
3. 下载对应平台的可执行文件
4. 根据需要赋予执行权限（Linux/macOS）：
   ```bash
   chmod +x fscan_linux_amd64
   ```

## ⚙️ 工作流程

### 自动化流程

1. **检查更新**：每天定时检查上游 fscan 仓库是否有新的 Git Tag
2. **版本对比**：将上游最新版本与本仓库最新 Release 版本进行对比
3. **触发编译**：如果发现新版本，自动触发编译流程
4. **多平台编译**：使用 Go 交叉编译生成多平台可执行文件
5. **文件压缩**：对支持的平台使用 UPX 进行压缩
6. **发布 Release**：自动创建 GitHub Release 并上传所有编译好的文件

### 手动触发

除了定时任务，你也可以手动触发编译：

1. 进入 Actions 标签页
2. 选择 "自动编译并发布 fscan" 工作流
3. 点击 "Run workflow" 按钮

## 🔧 技术细节

### 编译参数

- **Go 版本**：1.24
- **编译参数**：`go build -ldflags="-s -w" -trimpath -o <output> main.go`
  - `-s -w`：移除符号表和调试信息，减小文件体积
  - `-trimpath`：移除绝对路径信息
- **压缩工具**：UPX（`upx -9` 最高压缩率）

### 环境配置

- **运行环境**：Ubuntu Latest
- **Go 版本**：1.24
- **上游仓库**：[shadow1ng/fscan](https://github.com/shadow1ng/fscan)

## 📋 使用说明

fscan 的详细使用方法请参考：
- [官方仓库](https://github.com/shadow1ng/fscan)
- [使用文档](https://github.com/shadow1ng/fscan#readme)

基本用法示例：
```bash
# 扫描单个 IP
./fscan -h 192.168.1.1

# 扫描 IP 段
./fscan -h 192.168.1.1/24

# 扫描指定端口
./fscan -h 192.168.1.1 -p 22,80,443,3389

# 详细模式
./fscan -h 192.168.1.1 -v
```

## 🤝 贡献

本项目主要是自动化编译工具。

对于 fscan 本身的功能建议，请前往 [上游项目](https://github.com/shadow1ng/fscan) 提交 Issue。

## 📄 许可证

本项目遵循上游 fscan 项目的许可证。编译产物的使用请遵循 fscan 的相关规定。

## ⚠️ 免责声明

本工具仅用于安全测试和教育目的。使用者应当遵守当地法律法规，不得用于非法用途。使用本工具所产生的一切后果由使用者自行承担。

## 🔗 相关链接

- [fscan 官方仓库](https://github.com/shadow1ng/fscan)
- [本项目 Releases](../../releases)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
