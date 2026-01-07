# 使用 GitHub Actions 构建 APK

本指南将帮助您使用 GitHub Actions 自动构建 Android APK。

## 📋 构建方式

### 方式一: 推送代码自动构建 (推荐)

1. **修改配置文件**: GitHub Actions 配置已更新为 `build-android-apk.yml`
2. **提交并推送代码**:
   ```bash
   git add .github/workflows/android.yml
   git commit -m "添加 APK 自动构建流程"
   git push origin master
   ```

3. **查看构建状态**:
   - 访问 GitHub 仓库的 "Actions" 标签页
   - 查看最新的构建工作流运行状态

4. **下载 APK**:
   - 构建完成后,在 Actions 页面点击工作流
   - 在 "Artifacts" 部分下载:
     - `app-debug`: Debug 版本 APK
     - `app-release`: Release 版本 APK

### 方式二: 手动触发构建

如果需要手动触发构建,可以修改 GitHub Actions 配置为支持手动触发。

## 🚀 创建 Release 并自动上传 APK

1. 在 GitHub 仓库中创建新的 Release:
   - 点击 "Releases" → "Draft a new release"
   - 填写版本标签 (如: v1.0.0)
   - 填写 Release 标题和描述
   - 点击 "Publish release"

2. GitHub Actions 会自动:
   - 构建 Debug 和 Release APK
   - 将 APK 自动上传到该 Release
   - 任何人都可以直接从 Release 下载 APK

## 📦 APK 输出位置

- **Debug APK**: `app/build/outputs/apk/debug/app-debug.apk`
- **Release APK**: `app/build/outputs/apk/release/app-release.apk`

## 🔍 查看构建日志

1. 访问仓库的 "Actions" 标签页
2. 点击具体的工作流运行记录
3. 查看每个步骤的详细日志输出

## ⚙️ 工作流配置说明

`.github/workflows/android.yml` 配置文件包含以下步骤:

1. **检出代码**: 获取最新代码
2. **设置 JDK 11**: 配置 Java 环境
3. **设置 Android SDK**: 安装必要的 Android 工具
4. **构建 Debug APK**: 运行 `./gradlew assembleDebug`
5. **构建 Release APK**: 运行 `./gradlew assembleRelease`
6. **上传构建产物**: 保存 APK 到 GitHub Artifacts
7. **上传到 Releases** (仅 Release 触发): 将 APK 附加到 GitHub Release

## 📌 注意事项

- 首次构建可能需要较长时间(约 5-10 分钟)
- 每次推送到 master 分支都会触发构建
- Release 构建会自动上传 APK 到 Release 页面
- Debug APK 可用于测试,Release APK 可用于发布

## 🛠️ 故障排除

### 构建失败
检查 Actions 日志中的错误信息,常见问题:
- 依赖下载失败: 重试工作流
- NDK 缺失: 工作流会自动处理
- 构建时间过长: 这是正常现象,特别是首次构建

### APK 签名
当前生成的 Release APK 未签名,如需签名:
1. 创建 keystore 文件
2. 在 GitHub Secrets 中添加签名配置
3. 修改 `app/build.gradle.kts` 添加签名配置

## 📚 相关链接

- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Android Gradle Plugin 文档](https://developer.android.com/studio/build)
- [项目 GitHub Releases](../../releases)
