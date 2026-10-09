# HookEuicc

[![Build & Release](https://github.com/git8e/HookEuicc/actions/workflows/build.yml/badge.svg)](https://github.com/git8e/HookEuicc/actions/workflows/build.yml)
[![Latest Release](https://img.shields.io/github/v/release/git8e/HookEuicc?label=release&color=blue)](https://github.com/git8e/HookEuicc/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/git8e/HookEuicc/total?color=green)](https://github.com/git8e/HookEuicc/releases)

解除某些软件对 eSIM 的要求，也可用于获取 eSIM 代码。

在某些不直接提供 eSIM 激活码的应用中，会将激活码复制到剪切板并弹出分享界面，方便提取。

## 功能特性

- 伪装系统支持 eSIM（`FEATURE_TELEPHONY_EUICC`），绕过应用对 eSIM 硬件的检测
- 将 `EuiccManager.isEnabled()` 返回 `true`，并绕过 EuiccService 可用性检测
- 拦截 `DownloadableSubscription` 的激活码生成/编码过程，自动捕获并复制 eSIM 激活码到剪切板
- 补全 `TelephonyManager.getCardIdForDefaultEuicc()` 返回，避免相关调用崩溃
- **OMAPI Bypass**：绕过 ARA、ARF 限制，通常用于无 ARA 的卡进行 OMAPI 访问

## 安装与使用

1. 在 [Releases](https://github.com/git8e/HookEuicc/releases/latest) 页面下载最新版 APK 并安装
2. 打开 [LSPosed](https://github.com/LSPosed/LSPosed)（或兼容 Xposed 框架），在模块中启用 **HookEuicc**
3. 勾选需要作用的应用，然后重启该应用（或重启手机）

> [!NOTE]
> 首次安装后，需要重启目标应用（或整个系统）使 Hook 生效。

### OMAPI Bypass

需要勾选 `com.android.se` 并重启手机（或执行 `su -c killall com.android.se`）。

## 从源码构建

```bash
# 要求：JDK 17、Android SDK (platform 35)
./gradlew assembleRelease
# 产物：app/build/outputs/apk/release/app-release.apk
```

本项目已配置 GitHub Actions 自动构建：

- 推送到 `main` 分支或提交 PR 时，自动构建并上传 APK 构建产物
- 推送形如 `v1.0.1` 的 tag 时，自动构建 APK 并发布到 [Releases](https://github.com/git8e/HookEuicc/releases)
- 也可以在工作流页面手动触发 `Build & Release`，填写 `version` 输入项（如 `v1.0.1`）直接发布

> [!TIP]
> 默认情况下 release 构建使用 debug 签名，因此任意构建机均可产出可安装的 APK。
> 如需使用自己的签名，在仓库根目录添加 `keystore.properties`（或在 CI 中通过 Secrets 写入）：
> ```properties
> storeFile=/path/to/keystore.jks
> storePassword=your_store_password
> keyAlias=your_alias
> keyPassword=your_key_password
> ```

## 项目结构

```
├── app/src/main/java/cn/unicorn369/HookEuicc.java   # 模块主逻辑（Hook 入口）
├── app/src/main/resources/META-INF/xposed/          # Xposed 模块配置（module.prop / scope.list）
├── .github/workflows/build.yml                      # GitHub Actions 构建与发布
└── app/build.gradle                                 # 应用构建配置
```

## 捐赠

如果您喜欢这个项目，可以向以下地址捐赠：

| 链 | 地址 |
| --- | --- |
| USDT (ERC20) | `0x4d62f0c2a5bd9358bcd58352b0a3efd60afcf180` |
| USDT (TRC20) | `TDJvmrzpS496wjyx7rTR4bediAvWvx1XLf` |

## 致谢

本项目基于 [Unicorn369/HookEuicc](https://github.com/Unicorn369/HookEuicc) 派生并继续维护，感谢原作者的工作。
