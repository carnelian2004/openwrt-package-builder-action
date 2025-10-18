# OpenWrt Package Builder

<p align="center">
  <a href="https://github.com/features/actions">
    <img src="https://img.shields.io/badge/Powered%20by-GitHub%20Actions-blue?logo=github-actions" alt="Powered by GitHub Actions">
  </a>
  <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/blob/master/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green.svg" alt="License">
  </a>
  <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/issues">
    <img src="https://img.shields.io/github/issues/Tokisaki-Galaxy/openwrt-package-builder-action" alt="GitHub issues">
  </a>
   <a href="https://github.com/Tokisaki-Galaxy/openwrt-package-builder-action/stargazers">
    <img src="https://img.shields.io/github/stars/Tokisaki-Galaxy/openwrt-package-builder-action" alt="GitHub stars">
  </a>
</p>

<p align="center">
  <a href="README.CN.md"><img src="https://img.shields.io/badge/简体中文-brightgreen.svg" alt="简体中文"></a>
  <a href="README.md"><img src="https://img.shields.io/badge/English-blue.svg" alt="English"></a>
  <a href="README.DE.md"><img src="https://img.shields.io/badge/Deutsch-orange.svg" alt="Deutsch"></a>
</p>

A simple, versatile, and out-of-the-box GitHub Actions workflow for automatically building any OpenWrt package. This project offers two build modes: **SDK Mode (Recommended)** and **Source Mode**, catering to different needs.

You just need to place your package source code in the repository, choose a workflow file, modify two variables, and it will automatically compile `.ipk` files for multiple target architectures.

## ✨ Features

-   **🚀 Easy to Use**: Just copy-paste the workflow file and modify two environment variables to get started.
-   **⚡️ Extremely Fast Builds (SDK Mode)**: Utilizes official pre-compiled SDKs to reduce build time from 30+ minutes to under 5 minutes.
-   **💡 Flexible & Powerful (Source Mode)**: Capable of building any package, including kernel modules, and supports development branches like `master`.
-   **⚙️ Highly Versatile**: Can be used to compile any type of OpenWrt package, such as LuCI applications, command-line tools, or kernel modules.
-   **🎯 Multi-Target Compilation**: Pre-configured for popular architectures like `x86_64`, `armv8`, and `mips_mt7621`, and easily customizable.
-   **📦 Automatic Artifacts**: Upon successful compilation, `.ipk` files are automatically uploaded as build artifacts for easy download.
-   **💬 Commit Comments**: (Optional) After all build jobs succeed, a comment is automatically posted on the corresponding commit with a link to the artifacts.

## 🤔 Which Build Mode Should I Choose?

This project provides two workflows. Please choose one based on your needs.

| Feature / Scenario | ✅ SDK Mode (Recommended) | ⚠️ Source Mode |
| :--- | :--- | :--- |
| **Build Speed** | **Extremely fast** (usually <  10mins) | Slow (first build > 40 mins, faster with cache) |
| **Suitable Package Types** | Most **LuCI apps** and **regular packages** | **All packages**, especially **kernel modules (`kmod-xxx`)** |
| **Supported OpenWrt Versions** | Only **official stable releases** (e.g., `v23.05.3`) | **All versions**, including the `master` development branch |
| **Workflow File** | `build-with-sdk.yml` | `build-from-source.yml` |

**In short: If you're unsure, or you're just building a LuCI application, choose the `SDK Mode`.**

## 🚀 How to Use

#### Example

If you need a reference, you can check out this [example repository](https://github.com/Tokisaki-Galaxy/luci-app-tailscale-community/.github/workflows/build.yml).

#### Step 1: Choose and Create the Workflow File

Based on your choice above, create the corresponding `.github/workflows/` directory and YAML file in your repository's root.

*   **For SDK Mode**: Create `.github/workflows/build.yml`
*   **For Source Mode**: Create `.github/workflows/build-nonSDK.yml`

#### Step 2: Copy the Workflow Code

Copy the complete code from the corresponding file in this repository.

*   ➡️ [**Code for `build-with-sdk.yml`**](build.yml)
*   ➡️ [**Code for `build-from-source.yml`**](build-nonSDK.yml)

#### Step 3: Modify the Configuration

Open the YAML file you created. You only need to modify **two** variables in the `env` section:

1.  **`PACKAGE_NAME`**:
    Change its value to your package's directory name. **This is crucial** and must exactly match the folder name of your package in the repository.

2.  **`OPENWRT_VERSION`**:
    Set the OpenWrt version you want to build against.
    *   **For SDK Mode**: This **must** be an official release tag, e.g., `v23.05.3`.
    *   **For Source Mode**: This can be any tag or branch name, e.g., `v23.05.3` or `master`.

> **Tip:** Don't forget to update the badge links at the top of your `README.md`! Replace `Tokisaki-Galaxy/openwrt-package-builder-action` with your own repository name.

#### Step 4: Check Your Makefile (Important)

Ensure that your package's `Makefile` correctly defines all dependencies. This workflow relies on `make defconfig` to automatically resolve and select them.

For example:
```makefile
define Package/my-cool-package
  # ...
  DEPENDS:=+libcurl +libjson-c +some-other-package
endef
```

#### Step 5: Commit and Run

Commit your code and the new workflow YAML file to GitHub. GitHub Actions will start running automatically.

You can view the build progress in the `Actions` tab of your repository. Once completed, you can find and download the `Artifacts` from the corresponding run page.

## 🔧 Customization

### Customizing Build Targets

If you need to build for other architectures, modify the `jobs.build.strategy.matrix.target` section in the workflow file. Simply add or edit entries following the existing format.

> **Note**: The `matrix` structure is slightly different between the two modes. Please refer to the format in each respective file.

### Customizing Triggers

You can change the `on` section at the top of the file to modify what triggers the workflow, such as running on `pull_request` or on a `schedule`.

---

## License

This project is licensed under the [MIT License](LICENSE).
