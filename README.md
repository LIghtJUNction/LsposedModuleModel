<div align="center">
  <h1>LSPosed / Xposed 开发资料索引</h1>
  <p><strong>把分散的中文入门文章集中到一个可检索的文档仓库。</strong></p>
  <p>概念 · Android Studio 配置 · 模块入口 · Hook 示例 · 调试</p>

  [![Stars](https://img.shields.io/github/stars/LIghtJUNction/LsposedModuleModel?style=flat-square)](https://github.com/LIghtJUNction/LsposedModuleModel/stargazers)
  [![Last commit](https://img.shields.io/github/last-commit/LIghtJUNction/LsposedModuleModel?style=flat-square)](https://github.com/LIghtJUNction/LsposedModuleModel/commits/master)
</div>

> [!IMPORTANT]
> 这个仓库是**教程归档与索引**，不是可直接编译的 LSPosed 模块模板。文档来自不同作者和时期；Gradle、Android Studio、LSPosed 与 Android API 会变化，照抄前应核对当前官方文档和依赖版本。

## 从哪里开始

```text
先理解作用域与模块入口
          ↓
建立最小 Android 工程
          ↓
以 compileOnly 引入 Xposed API
          ↓
声明模块元数据和入口
          ↓
只对测试应用做最小 Hook
          ↓
查看日志、缩小作用域、逐步验证
```

首次接触建议按这个顺序阅读：

1. [LSPosed official wiki 摘要](doc/official_wiki.MD) — 框架、作用域与官方开发者入口。
2. [从零开始编写 Xposed 模块](doc/luoboQAQ.MD) — Java/Kotlin 工程、依赖、作用域与完整实例。
3. [Android Studio 2021.3.1 模块编写指南](doc/scrawman.MD) — 传统 API 82、`xposed_init` 与调试流程。
4. [Xposed 课程笔记](doc/ZJ595.MD) — 原理、工具、逆向环境与常用 API 的长篇整理。

## 文档目录

| 文档 | 适合解决的问题 |
| --- | --- |
| [`official_wiki.MD`](doc/official_wiki.MD) | LSPosed 与传统 Xposed 的差异、用户/开发者官方链接 |
| [`luoboQAQ.MD`](doc/luoboQAQ.MD) | 从空项目到第一个可工作的 Java/Kotlin 模块 |
| [`scrawman.MD`](doc/scrawman.MD) | Android Studio、API 82、Manifest、`xposed_init` 与设备调试 |
| [`ZJ595.MD`](doc/ZJ595.MD) | 系统化课程笔记、Hook 原理、工具与逆向环境 |
| [`smice.MD`](doc/smice.MD) | 另一份模块开发实践记录 |
| [`狐言狐语和仙贝的魔法学习记录.MD`](doc/狐言狐语和仙贝的魔法学习记录.MD) | 较完整的中文学习记录与示例 |

文档名称保留原归档形式，便于追溯和搜索；仓库没有把这些文章重新包装成统一“官方教程”。

## 先理解三个东西

### 1. 模块本质

Xposed/LSPosed 模块本质上是一个 Android APK。框架在目标进程加载模块入口，模块再寻找并替换目标类、方法或字段的行为。

```text
目标应用启动
    ↓
LSPosed 判断该应用是否在模块作用域
    ↓
加载模块入口
    ↓
模块定位类/方法
    ↓
方法执行前、执行后或完全替换时运行自定义逻辑
```

### 2. `compileOnly` 的意义

Xposed API 由运行时框架提供。模块只需要在编译时知道这些类的接口，不应把 API 类打进 APK：

```gradle
dependencies {
    compileOnly 'de.robv.android.xposed:api:82'
}
```

这就是 `compileOnly`：**编译时可见，打包时不复制**。若使用 `implementation`，可能出现“Xposed API classes are compiled into the module's APK”一类加载错误。

### 3. 作用域不是装饰

LSPosed 只向用户勾选的目标进程注入模块。作用域越小，冲突面、性能影响和误伤风险越小。开发阶段应只选择自己的测试应用，不要一开始勾选系统框架和全部应用。

## 现代项目建议

旧教程仍能解释底层结构，但新项目优先考虑维护中的开发框架和模板，例如：

- [YukiHookAPI 文档](https://highcapable.github.io/YukiHookAPI/zh-cn/)
- [YukiHookAPI Project Builder](https://highcapable.github.io/YukiHookAPI/zh-cn/tools/yukihookapi-projectbuilder)
- [LSPosed 官方 Wiki](https://github.com/LSPosed/LSPosed/wiki)

使用新框架并不会改变核心机制：最终仍是声明入口、限定作用域、定位目标代码、安装 Hook，并在目标进程中验证行为。

## 使用这些旧文档时要检查什么

- Gradle 仓库是否仍可用，避免继续添加已经停止维护的 `jcenter()`。
- Android Gradle Plugin 与 Gradle 语法是否已变化。
- Manifest 元数据、现代模块入口和 LSPosed 推荐方式是否更新。
- 被 Hook 应用的类名、混淆结果和方法签名是否仍一致。
- Android 隐私、进程隔离、SELinux 和目标 SDK 限制是否改变。
- 示例是否只用于你有权测试的应用与设备。

Hook 代码“编译通过”只证明语法成立。真正的验证链是：模块被框架识别、作用域正确、目标进程加载入口、类/方法匹配、回调执行、目标功能没有崩溃。

## 归档与版权

`doc/` 中的文章保留各自标题、署名线索、原始链接和内容形态。它们不因被收录到本仓库而自动获得新的许可。引用、转载或二次发布时，应回到原始来源核对作者与授权条件。

本仓库没有提供一个统一许可证，也不代表 LSPosed 官方维护这些归档内容。
