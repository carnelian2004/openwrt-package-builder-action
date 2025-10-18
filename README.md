# OpenWrt Package Builder


一个简单、通用且开箱即用的 GitHub Actions 工作流，用于自动编译任何 OpenWrt 软件包（包括 LuCI 应用）。

你只需要将你的软件包源码放在仓库中，复制一份工作流文件，修改两个变量，即可为多个目标架构自动编译出 `.ipk` 文件。

## ✨ 特性

-   **🚀 简单易用**: 只需复制粘贴工作流文件，修改两个环境变量即可开始使用。
-   **⚙️ 高度通用**: 可用于编译任何类型的 OpenWrt 软件包，不仅仅是 LuCI 应用。
-   **🎯 多目标编译**: 默认配置为 `x86_64`, `armv8`, `mips_mt7621` 三个常用架构，可轻松自定义。
-   **⚡️ 高效缓存**: 智能缓存 OpenWrt 编译环境，显著加快后续构建速度。
-   **📦 自动发布**: 编译成功后，自动将 `.ipk` 文件上传为构建产物 (Artifacts)，方便下载。
-   **💬 提交评论**: (可选) 所有构建任务成功后，会在对应的 Commit 下自动发表评论，提供产物下载链接。

## 🚀 如何使用

#### 步骤 1: 创建工作流文件

在你的软件包仓库根目录下，创建一个名为 `.github/workflows/build.yml` 的文件。

#### 步骤 2: 复制工作流代码

将本仓库下的build.yml代码复制过去。


#### 步骤 3: 修改配置

打开 `build.yml` 文件，你只需要修改 `env` 部分的 **两个** 变量：

1.  **`PACKAGE_NAME`**:
    将其值修改为你的软件包的目录名。**非常重要**，这个名字必须和你的软件包在仓库中的文件夹名称完全一致。

2.  **`OPENWRT_VERSION`**:
    设置你想要基于哪个 OpenWrt 版本进行编译。可以是稳定的标签（如 `v23.05.3`），也可以是开发分支（如 `master`）。

#### 步骤 4: 检查 Makefile (重要)

确保你的软件包目录下的 `Makefile` 文件中正确定义了所有依赖项。本工作流依赖 `make defconfig` 来自动解析并选中这些依赖。

例如:
```makefile
define Package/my-cool-package
  # ...
  DEPENDS:=+libcurl +libjson-c +some-other-package
endef
```

#### 步骤 5: 提交并运行

将你的代码和 `.github/workflows/build.yml` 文件提交并推送到 GitHub。GitHub Actions 会自动开始运行。

你可以在仓库的 `Actions` 标签页查看编译进度。编译完成后，可以在对应的运行记录页面找到并下载 `Artifacts`。

## 🔧 自定义

### 修改编译目标

如果你需要为其他架构编译，可以修改 `jobs.build.strategy.matrix.target` 部分。只需按照现有格式添加或修改条目即可。你可以在 OpenWrt 源码的 `target/linux/` 目录下找到所有可用的 `target` 和 `subtarget`。

### 修改触发条件

你可以修改文件顶部的 `on` 部分来更改触发工作流的条件，例如改为 `pull_request` 或定时运行 (`schedule`)。

---

## 协议

本项目使用 [MIT License](LICENSE) 授权。