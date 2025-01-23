在 Windows 上编译 `mpv` 以生成原生 Windows 可执行文件，通常使用 **MSYS2** 环境与 **MinGW** 工具链。以下是详细的步骤：

### 1. 安装 MSYS2
MSYS2 提供了一个类似 Linux 的开发环境，支持使用 MinGW 工具链编译 Windows 程序。

- 访问 [MSYS2 官网](https://www.msys2.org/) 下载并安装 MSYS2。
- 安装过程中，确保按提示更新 MSYS2 的核心包：
  ```bash
  pacman -Syu
  ```
  更新完毕后，关闭并重新打开 MSYS2 终端。

### 2. 安装编译工具和依赖项
首先，安装 MinGW 工具链和其他编译 `mpv` 所需的依赖库。

- 安装 MinGW 编译工具：
  ```bash
  pacman -S mingw-w64-x86_64-toolchain
  ```
  这会安装 64 位的编译工具链。

- 安装其他依赖项，如 `yasm`（汇编工具）、`pkg-config`、`ninja` 等：
  ```bash
  pacman -S yasm pkg-config ninja
  ```

- 安装 `ffmpeg` 相关依赖（`libavcodec`、`libavutil` 等）和其他库：
  ```bash
  pacman -S mingw-w64-x86_64-ffmpeg mingw-w64-x86_64-libass mingw-w64-x86_64-fontconfig mingw-w64-x86_64-freetype
  ```

### 3. 克隆 `mpv` 源代码
使用 Git 克隆 `mpv` 的源代码：

```bash
git clone https://github.com/mpv-player/mpv.git
cd mpv
```

### 4. 配置构建环境
在 `mpv` 项目的根目录中使用 **Meson** 来设置构建环境。

- 使用 Meson 设置构建目录：
  ```bash
  meson setup build --prefix=/mingw64
  ```

  `--prefix=/mingw64` 选项指定了安装路径，`/mingw64` 是 MSYS2 环境中用于 64 位 MinGW 的默认路径。

### 5. 编译 `mpv`
使用 **Ninja** 来开始构建 `mpv`：

```bash
ninja -C build
```

这将编译 `mpv` 项目并生成可执行文件。如果您希望编译特定的目标或特性（例如支持 `ffmpeg`），可以在构建配置时指定相应选项。

### 6. 安装 `mpv`
编译完成后，可以将 `mpv` 安装到 MSYS2 的 MinGW 环境中：

```bash
ninja -C build install
```

这将在 MSYS2 中的 `/mingw64/bin` 目录下生成 `mpv.exe`，您可以在 MSYS2 环境内运行它。

### 7. 设置环境变量（可选）
为了在 Windows 的命令提示符或 PowerShell 中直接运行 `mpv`，可以将 `mpv.exe` 的路径添加到系统的环境变量中。

- 打开 **系统属性** -> **高级系统设置** -> **环境变量**。
- 在 **系统变量** 中找到 `Path`，并将 `C:\msys64\mingw64\bin` 添加到其中。

### 8. 运行 `mpv`
完成安装后，您可以通过 MSYS2 终端或命令提示符（如果已设置环境变量）直接运行 `mpv`：

```bash
mpv <video_url>
```

### 9. 调试与验证
在编译过程中，如果遇到任何依赖项问题（如找不到库或头文件），可以使用 `pkg-config` 来检查库的状态。比如：
```bash
pkg-config --cflags --libs libavcodec
```

### 其他注意事项
- **启用 `ffmpeg` 支持**：确保在配置时选择启用 `ffmpeg` 支持（如果需要），`mpv` 依赖于 `ffmpeg` 来处理视频和音频编解码。
- **检查缺失的依赖**：如果编译失败并报告缺少库或工具，可以通过 `pacman -S` 安装缺失的包。
- **调试构建过程**：可以使用 `ninja -v` 查看详细的构建过程和命令输出，以帮助调试。

### 总结
通过上述步骤，您可以在 Windows 上使用 MSYS2 和 MinGW 编译原生 Windows 版本的 `mpv`。这个过程相对直接，尤其适合开发者需要在 Windows 平台上定制和编译 `mpv` 的场景。
