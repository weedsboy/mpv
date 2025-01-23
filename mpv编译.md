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




要使用 **Meson** 来设置和编译 **libmpv**，并指定安装路径（例如 `/mingw64`），需要按以下步骤操作：

### 1. 获取 libmpv 源代码

首先，您需要获取 libmpv 的源代码。您可以从 **mpv** 的 GitHub 仓库克隆代码：

```bash
git clone https://github.com/mpv-player/mpv.git
cd mpv
```

### 2. 确保安装依赖项

在 Windows 上使用 **MSYS2** 和 **MinGW** 构建 **mpv** 需要一些必要的依赖项。确保您已安装 **libmpv** 构建所需的库。

打开 **MSYS2 MinGW 64-bit Shell**，并安装必需的包：

```bash
pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-meson mingw-w64-x86_64-ninja mingw-w64-x86_64-libmpv
```

这些包包括：

- `mingw-w64-x86_64-gcc`：GCC 编译器。
- `mingw-w64-x86_64-meson`：Meson 构建系统。
- `mingw-w64-x86_64-ninja`：Ninja 构建工具。
- `mingw-w64-x86_64-libmpv`：libmpv 相关的依赖库。

如果缺少其他依赖（例如 **FFmpeg**、**libass**、**libbluray** 等），请安装相应的库。

### 3. 配置 Meson 构建目录

进入 `mpv` 项目的根目录，使用 **Meson** 配置构建目录，并设置安装路径为 `/mingw64`。运行以下命令：

```bash
meson setup build --prefix=/mingw64 --buildtype debug
```

这里的参数说明：

- `build`: 构建目录（Meson 将在此目录中创建构建文件）。
- `--prefix=/mingw64`: 指定安装路径为 `/mingw64`，即将安装到 **MinGW** 的 64 位系统目录。
- `--buildtype debug`: 使用调试构建类型，生成调试符号（如果需要）。

如果您不需要调试符号，可以使用 `--buildtype release` 以生成优化过的发布版本。

### 4. 编译 libmpv

配置完成后，使用 **Ninja** 来编译项目：

```bash
ninja -C build
```

这将开始编译 **libmpv**。 `-C` 参数指示 **Ninja** 在 `build` 目录中进行构建。

### 5. 安装 libmpv（可选）

编译完成后，您可以选择将 **libmpv** 安装到指定的目录（在本例中为 `/mingw64`）。运行以下命令进行安装：

```bash
ninja -C build install
```

这会将构建的文件安装到您在 `--prefix` 参数中指定的路径（例如 `/mingw64`）中。

### 6. 验证安装

您可以检查安装是否成功，确认 **libmpv** 库文件是否位于指定的安装路径中。可以通过以下命令检查 `libmpv` 的安装：

```bash
ls /mingw64/lib
```

### 7. 使用 libmpv

安装完成后，您可以在 **C++ 项目** 中使用 **libmpv**，并确保在编译和链接时将 `/mingw64/include` 和 `/mingw64/lib` 目录包含在内。例如：

```bash
g++ -o your_program your_program.cpp -I/mingw64/include -L/mingw64/lib -lmpv
```

### 解决常见问题

- 如果编译过程中遇到缺失依赖或错误，确保 **MSYS2** 环境中安装了所有相关依赖包。
- 如果 `meson setup` 命令遇到问题，可以尝试清理构建目录并重新配置：
  ```bash
  meson setup build --reconfigure --prefix=/mingw64
  ```

### 总结

通过以上步骤，您应该能够在 Windows 上使用 **Meson** 和 **MinGW** 编译并安装 **libmpv**。如果遇到问题或有任何疑问，请随时告诉我！

meson setup build --prefix=/mingw64
pacman -S mingw-w64-x86_64-gcc mingw-w64-x86_64-meson mingw-w64-x86_64-ninja mingw-w64-x86_64-libmpv
meson setup build --prefix=/mingw64 --buildtype debug
meson setup build --prefix=/mingw64 --buildtype debug --reconfigure
meson configure build -Dlibmpv=true -Ddefault_library=static
meson configure build -Dlibmpv=true -Ddefault_library=shared
ninja -C build libmpv-2.dll
