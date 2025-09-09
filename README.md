# Flutter GetX MVVM Template

一个基于 Flutter + GetX 的 MVVM 架构模板项目，提供完整的项目结构和开发规范，帮助开发者快速构建高质量的 Flutter 应用。

## 📱 项目简介

本项目是一个完整的 Flutter 应用模板，采用 GetX 状态管理和 MVVM 架构模式。项目包含了现代移动应用开发的最佳实践，包括国际化支持、主题切换、网络请求、本地存储等功能。

### ✨ 主要特性

- 🏗️ **MVVM 架构**: 清晰的代码分层，易于维护和扩展
- 🎯 **GetX 状态管理**: 高性能的响应式状态管理解决方案
- 🌍 **国际化支持**: 内置多语言支持（英语、孟加拉语）
- 🎨 **主题系统**: 支持亮色/暗色主题切换
- 🌐 **网络层**: 基于 Dio 的完整网络请求封装
- 💾 **本地存储**: SharedPreferences 数据持久化
- 🔧 **环境配置**: 支持开发/生产环境配置
- 📱 **响应式设计**: 适配不同屏幕尺寸
- 🎭 **自然界面设计**: 避免过度 AI 生成感，保持和谐视觉效果

## 🛠️ 技术栈

### 核心框架
- **Flutter**: 3.13.0+
- **Dart**: 3.1.0+

### 主要依赖
- **get**: ^4.6.6 - 状态管理、路由管理、依赖注入
- **dio**: ^5.3.3 - HTTP 网络请求
- **cached_network_image**: ^3.3.0 - 网络图片缓存
- **shared_preferences**: ^2.2.2 - 本地数据存储
- **flutter_svg**: ^2.0.8 - SVG 图片支持
- **font_awesome_flutter**: ^10.6.0 - 图标库
- **fluttertoast**: ^8.2.2 - 消息提示
- **logger**: ^2.0.2+1 - 日志记录
- **pretty_dio_logger**: ^1.3.1 - 网络请求日志
- **animations**: ^2.0.8 - 动画效果

### 开发工具
- **flutter_lints**: ^2.0.2 - 代码规范检查
- **dart_code_metrics**: 代码质量分析
- **analyzer**: ^5.13.0 - 静态代码分析

## 📁 项目结构

```
flutter_getx_template/
├── android/                 # Android 平台文件
├── ios/                     # iOS 平台文件
├── web/                     # Web 平台文件
├── lib/                     # 主要源代码
│   ├── app/
│   │   ├── bindings/        # 全局依赖注入
│   │   ├── core/            # 核心基础组件
│   │   │   ├── base/        # 基础抽象类
│   │   │   ├── model/       # 核心数据模型
│   │   │   ├── utils/       # 工具类
│   │   │   ├── values/      # 常量定义
│   │   │   └── widget/      # 通用 UI 组件
│   │   ├── data/            # 数据层
│   │   │   ├── local/       # 本地数据源
│   │   │   ├── model/       # 数据模型
│   │   │   ├── remote/      # 远程数据源
│   │   │   └── repository/  # 数据仓库
│   │   ├── modules/         # 功能模块
│   │   │   ├── home/        # 首页模块
│   │   │   ├── favorite/    # 收藏模块
│   │   │   ├── settings/    # 设置模块
│   │   │   ├── main/        # 主页面模块
│   │   │   ├── other/       # 其他模块
│   │   │   └── project_details/ # 项目详情模块
│   │   ├── network/         # 网络层配置
│   │   ├── routes/          # 路由配置
│   │   └── my_app.dart      # 应用入口
│   ├── flavors/             # 环境配置
│   ├── l10n/                # 国际化文件
│   ├── main_dev.dart        # 开发环境入口
│   └── main_prod.dart       # 生产环境入口
├── fonts/                   # 字体资源
├── images/                  # 图片资源
├── test/                    # 测试文件
├── repo_data/               # 项目展示图片
└── pubspec.yaml             # 项目配置文件
```

## 🚀 快速开始

### 环境要求

- Flutter SDK: 3.13.0 或更高版本
- Dart SDK: 3.1.0 或更高版本
- Android Studio / VS Code
- iOS 开发需要 Xcode（仅 macOS）

### 安装步骤

1. **克隆项目**
   ```bash
   git clone <repository-url>
   cd flutter_getx_template
   ```

2. **安装依赖**
   ```bash
   flutter pub get
   ```

3. **生成国际化文件**
   ```bash
   flutter gen-l10n
   ```

4. **运行项目**
   
   开发环境：
   ```bash
   flutter run --target lib/main_dev.dart
   ```
   
   生产环境：
   ```bash
   flutter run --target lib/main_prod.dart
   ```

### 构建发布版本

**Android APK:**
```bash
flutter build apk --target lib/main_prod.dart --release
```

**iOS IPA:**
```bash
flutter build ios --target lib/main_prod.dart --release
```

**Web:**
```bash
flutter build web --target lib/main_prod.dart --release
```

## 📖 开发指南

### MVVM 架构说明

本项目采用 MVVM（Model-View-ViewModel）架构模式：

- **Model**: 数据模型和业务逻辑（`lib/app/data/`）
- **View**: UI 界面（`lib/app/modules/*/views/`）
- **ViewModel**: 控制器，连接 Model 和 View（`lib/app/modules/*/controllers/`）

### 添加新功能模块

1. 在 `lib/app/modules/` 下创建新模块目录
2. 按照以下结构创建文件：
   ```
   new_module/
   ├── bindings/
   │   └── new_module_binding.dart
   ├── controllers/
   │   └── new_module_controller.dart
   └── views/
       └── new_module_view.dart
   ```

3. 在 `lib/app/routes/` 中添加路由配置

### 状态管理

使用 GetX 进行状态管理：

```dart
// Controller
class HomeController extends GetxController {
  final RxList<Item> items = <Item>[].obs;
  
  void addItem(Item item) {
    items.add(item);
  }
}

// View
class HomeView extends GetView<HomeController> {
  @override
  Widget build(BuildContext context) {
    return Obx(() => ListView.builder(
      itemCount: controller.items.length,
      itemBuilder: (context, index) => ItemWidget(controller.items[index]),
    ));
  }
}
```

### 网络请求

```dart
// Repository
class ApiRepository {
  final ApiClient _apiClient;
  
  Future<List<Item>> getItems() async {
    final response = await _apiClient.get('/items');
    return (response.data as List)
        .map((json) => Item.fromJson(json))
        .toList();
  }
}
```

### 国际化

1. 在 `lib/l10n/app_*.arb` 文件中添加翻译
2. 运行 `flutter gen-l10n` 生成代码
3. 在代码中使用：
   ```dart
   Text(AppLocalizations.of(context)!.hello)
   ```

## 🎯 功能模块说明

### 主要模块

- **Home**: 首页，展示 GitHub 项目列表
- **Favorite**: 收藏页面，管理收藏的项目
- **Settings**: 设置页面，包含主题切换、语言设置等
- **Project Details**: 项目详情页面
- **Main**: 主导航页面，底部导航栏
- **Other**: 其他功能页面

### 核心功能

- **主题切换**: 支持亮色/暗色主题
- **多语言**: 支持英语和孟加拉语
- **网络请求**: GitHub API 集成
- **图片缓存**: 网络图片自动缓存
- **本地存储**: 用户偏好设置持久化
- **消息提示**: 统一的消息提示系统

## 📝 代码规范

项目遵循严格的代码规范，详细规范请参考 `.trae/documents/Flutter_GetX_MVVM_开发规范文档.md`。

### 主要规范要点

- **命名约定**: 文件使用 snake_case，类使用 PascalCase
- **代码结构**: 统一的类结构和导入顺序
- **注释规范**: 所有公共 API 必须有文档注释
- **异常处理**: 分层的异常处理机制
- **界面设计**: 避免过度 AI 生成感，保持自然和谐
