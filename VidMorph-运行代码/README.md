# 视频 Token 基因提取引擎 (Video Token Profile Engine)

## 🔐 安全特性说明

### 绝对安全设计
- **无文件记忆**: API密钥永远不会被保存到任何本地文件
- **纯内存存储**: 密钥仅存在于当前进程的内存环境变量中
- **自动清除**: 程序关闭后密钥立即从内存消失
- **图形化输入**: 使用安全密码框形式输入，默认隐藏显示

### 安全使用流程

1. **启动安全输入程序**
   ```bash
   python scripts/secure_api_launcher.py
   ```

2. **输入API密钥**
   - 在弹出的图形窗口中输入您的API密钥
   - 密钥默认以星号(*)隐藏
   - 可勾选"显示密钥"临时查看确认

3. **在其他模块中使用**
   - 所有需要API的分析模块，从环境变量中读取：
   ```python
   import os
   api_key = os.environ.get("TEMP_API_KEY")
   ```

## 项目功能

- 抖音视频自动分析
- 视频节奏可视化
- 视频时长分布分析
- 固定 24 维 Video Token Profile 提取
- 用户图片、视频与文本素材适配分析
- 基于已有素材的拍同款改进方案生成

## 目录说明

- `src/`: 核心源码目录
  - `src/core/`: 大模型调用与 24 维 Token Profile 编排逻辑
  - `src/types/`: Video Token Profile 数据类型定义
  - `src/ui/`: 本地 GUI 入口与可视化交互相关代码
- `scripts/`: 本地测试脚本与安全输入工具
- `mock/`: 示例数据与测试素材目录
- `reports/`: 本地生成的分析报告输出目录

> `reports/` 属于运行产物目录，默认不提交到 GitHub。第一步事实报告会导出为 PDF，拍同款改进方案会导出为独立 JSON 文件。

## 安装依赖

建议新用户先按下面的完整流程配置环境。

### 新用户从零开始运行

以下步骤以 Windows 为主。如果你把整个项目文件夹发给别人，对方解压后按顺序执行即可。

1. 安装 Python

   - 建议安装 Python 3.10 或更高版本。
   - Windows 安装时请勾选 `Add python.exe to PATH`。
   - 安装后打开 PowerShell，确认命令可用：

   ```bash
   python --version
   ```

   如果系统没有 `python` 命令，也可以尝试：

   ```bash
   py --version
   ```

2. 进入项目目录

   假设项目文件夹放在 `D:\爆款视频`，在 PowerShell 中执行：

   ```bash
   cd D:\爆款视频
   ```

3. 创建并启用虚拟环境

   ```bash
   python -m venv .venv
   .\.venv\Scripts\activate
   ```

   如果你使用的是 `py` 命令：

   ```bash
   py -m venv .venv
   .\.venv\Scripts\activate
   ```

   启用成功后，命令行前面通常会出现 `(.venv)`。

4. 安装 Python 依赖

   ```bash
   python -m pip install --upgrade pip
   python -m pip install -r requirements.txt
   ```

   依赖包含：

   - `opencv-python`：读取视频基础信息与时长兜底；
   - `matplotlib`：导出 PDF 报告；
   - `aiohttp`：调用模型 API；
   - `tkinterdnd2`：支持拖拽上传视频。

5. 安装或确认 ffprobe

   程序会优先使用 `ffprobe` 读取视频真实总时长。如果没有安装，也会尝试使用 OpenCV 兜底；但为了更稳定，建议安装 FFmpeg，并确认 `ffprobe` 可用：

   ```bash
   ffprobe -version
   ```

   如果提示找不到命令，请安装 FFmpeg，并把 FFmpeg 的 `bin` 目录加入系统 `PATH`。

6. 准备火山方舟 API Key

   启动 GUI 后，在界面里输入 API Key。API Key 只在当前程序内存中使用，不要写入源码、README、测试文件或任何提交到仓库的文件。

7. 启动桌面 GUI

   ```bash
   python main.py
   ```

   如果你的系统使用 `py`：

   ```bash
   py main.py
   ```

8. 使用流程

   - 在 GUI 中输入火山方舟 API Key；
   - 拖拽或选择 `.mp4`、`.mov`、`.avi`、`.mkv`、`.webm` 视频；
   - 点击启动分析；
   - 等待 24 个 Video Token 校验通过；
   - 在 JSON 预览中确认 `videoTokensProfile`；
   - 点击导出数据报告，PDF 会保存到你选择的位置，默认可放在 `reports/`。
   - 切换到“用户素材分析”，上传图片、视频或文本素材；
   - 运行素材分析，确认每份素材适合放入哪些 Token 和时间段；
   - 切换到“改进方案生成”，生成已有素材复用、剪辑、补拍或 AI 生成建议；
   - 点击导出方案，JSON 文件会保存到你选择的位置。

9. 常见问题

   - 如果提示缺少 `matplotlib`、`cv2` 或 `aiohttp`，请确认虚拟环境已启用，并重新执行：

   ```bash
   python -m pip install -r requirements.txt
   ```

   - 如果提示无法读取视频真实时长，请优先安装 FFmpeg 并确认 `ffprobe -version` 可正常输出。
   - 如果 API 调用失败，请检查 API Key 是否正确、网络是否可访问火山方舟接口。
   - `reports/` 和 `*.pdf` 是本地运行产物，默认不会提交到 Git。

### 快速命令汇总

```bash
cd D:\爆款视频
python -m venv .venv
.\.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python main.py
```

## 启动本地 GUI

```bash
python main.py
```

如果 Windows 上没有 `python` 命令，可使用：

```bash
py main.py
```

启动后可将 `.mp4`、`.mov`、`.avi`、`.mkv`、`.webm` 视频文件直接拖拽到上传区域，也可以点击上传区域或“浏览...”按钮选择本地视频。

## 重要提醒
- 永远不要在代码中硬编码API密钥
- 每次使用程序都需要重新输入密钥
- 本项目已配置完善的`.gitignore`，防止敏感文件意外提交
