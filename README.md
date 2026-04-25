# Qwen3-TTS on Android (ZeroTermux)

An experimental project to deploy and run Qwen3-TTS WebUI on Android devices using ZeroTermux and a proot-based Ubuntu environment.

**⚠️ Status: Unstable / Proof-of-Concept**

This setup is not plug-and-play. Environment conflicts, memory constraints, and dependency mismatches are common. It works, but not smoothly. Contributions and improvements are warmly welcomed.

---

## Hardware Requirements (Empirical, not official)

| Model Version | Physical RAM (minimum) | Recommended RAM | Storage Required |
| :--- | :--- | :--- | :--- |
| Qwen3-TTS 0.6B | **12 GB** | 16 GB+ | ≈ 10 GB |
| Qwen3-TTS 1.7B | **24 GB** | 32 GB+ | ≈ 12 GB |

> **Important Notes:**
> - These figures are based on real-world testing on Android.
> - **Memory extension / RAM expansion / swap does NOT help** — only your device's actual physical RAM matters.
> - The 0.6B model could not complete inference on a 6 GB physical RAM device, hence the 12 GB minimum recommendation.

---

## Known Issues

- Environment instability (dependency conflicts require manual fixes).
- High memory usage (WebUI + Python runtime consume significant RAM before model loading).
- Slow inference (CPU-only on mobile).
- Not optimized for mobile (desktop-style Python environment, not a native Android app).

---

## Looking for Help

If you have ideas to make this more stable, lightweight, or reproducible — please open an issue or submit a PR. Areas that need improvement:

- Porting to a native Android inference framework (e.g., MNN, llama.cpp).
- Using quantized GGUF models to reduce memory footprint.
- Simplifying the setup process.
- Testing on a wider range of Android devices.

---

## License

MIT

---

# Qwen3-TTS 安卓部署 (ZeroTermux)

使用 ZeroTermux 和 proot 搭建的 Ubuntu 环境，在安卓手机上部署并运行 Qwen3-TTS WebUI 的实验性项目。

**⚠️ 状态：不稳定 / 概念验证**

本项目并非开箱即用。环境冲突、内存不足、依赖不兼容等问题较为常见。项目能够运行，但不够流畅。欢迎贡献和改进。

---

## 硬件要求（实测，非官方数据）

| 模型版本 | 物理内存（最低） | 推荐物理内存 | 存储需求 |
| :--- | :--- | :--- | :--- |
| Qwen3-TTS 0.6B | **12 GB** | 16 GB+ | ≈ 10 GB |
| Qwen3-TTS 1.7B | **24 GB** | 32 GB+ | ≈ 12 GB |

> **重要说明：**
> - 以上数据基于安卓手机实测。
> - **内存融合 / 内存扩展 / swap 没有帮助** — 只有设备的实际物理内存才有意义。
> - 0.6B 模型在 6 GB 物理内存设备上无法完成推理，因此给出 12 GB 的最低建议。

---

## 已知问题

- 环境不稳定（依赖冲突需要手动修复）。
- 内存占用高（WebUI 和 Python 运行时在加载模型前已消耗大量内存）。
- 推理速度慢（手机端仅 CPU 运算）。
- 未针对移动端优化（桌面级 Python 环境，非原生 Android 应用）。

---

## 寻求帮助

如果你有办法让本项目更稳定、更轻量、更易复现 — 欢迎提交 Issue 或 PR。需要改进的方向：

- 迁移到原生 Android 推理框架（如 MNN、llama.cpp）。
- 使用 GGUF 量化模型降低内存占用。
- 简化安装流程。
- 在更多安卓设备上测试。

＃＃运行方法
下载termux或zerotermux
termux：https://github.com/termux/termux-app
zerotermux：https://github.com/hanxinhao000/ZeroTermux
先输入“pkg update && pkg upgrade”执行更新
硬性条件：运存12G（最低0.6B模型），内存30G。

1.pkg install -y git curl wget unzip python python-pip clang make cmake proot-distro（安装所有需要的工具和软件）

2.proot-distro install ubuntu（安装Ubuntu子系统）

3.proot-distro login ubuntu（进入Ubuntu环境）

4.apt update && apt upgrade -y（更新Ubuntu系统）

5.apt install -y git python3 python3-pip python3-venv（安装git和Python相关工具）

6.export http_proxy=http://127.0.0.1:7890（设置HTTP代理，端口改成你自己的）

7.export https_proxy=http://127.0.0.1:7890（设置HTTPS代理）

8.git clone https://github.com/licyk/qwen-tts-webui.git（下载项目代码）

9.cd qwen-tts-webui（进入项目目录）

10.python3 -m venv venv（创建虚拟环境）

11.source venv/bin/activate（激活虚拟环境）

12.pip install torch torchaudio --index-url https://download.pytorch.org/whl/cpu（安装PyTorch CPU版）

13.pip install gradio soundfile -i https://pypi.tuna.tsinghua.edu.cn/simple（安装网页界面和音频处理库）

14.pip install tokenizers==0.15.2（安装分词器，指定版本避免编译出错）

15.pip install transformers==4.43.0（安装模型库，指定版本避免冲突）

16.pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple（安装项目所有依赖）

17.pip uninstall torchvision -y（卸载torchvision，TTS不需要它还导致冲突）

18.mkdir -p venv/lib/python3.13/site-packages/torchvision/transforms（创建假torchvision目录，骗过系统）

19.touch venv/lib/python3.13/site-packages/torchvision/__init__.py（创建空文件让Python识别为包）

20.touch venv/lib/python3.13/site-packages/torchvision/transforms/__init__.py（同上，补全子目录）

21.pip install transformers==4.57.3（安装兼容版本，根除torchvision报错）

22.python launch.py --modelscope（启动WebUI，模型从国内源下载）
希望大佬们给出优化方案
