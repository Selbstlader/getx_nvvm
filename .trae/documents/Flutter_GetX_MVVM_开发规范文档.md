# Flutter GetX MVVM 开发规范文档

## 1. 文件组织结构说明

### 1.1 项目根目录结构
```
flutter_getx_template/
├── android/                 # Android平台相关文件
├── ios/                     # iOS平台相关文件
├── web/                     # Web平台相关文件
├── lib/                     # 主要源代码目录
├── test/                    # 测试文件目录
├── fonts/                   # 字体资源文件
├── images/                  # 图片资源文件
├── l10n.yaml               # 国际化配置文件
├── pubspec.yaml            # 项目依赖配置
├── analysis_options.yaml   # 代码分析配置
└── README.md               # 项目说明文档
```

### 1.2 lib目录结构（核心架构）
```
lib/
├── app/
│   ├── bindings/           # 依赖注入绑定
│   ├── core/               # 核心基础组件
│   │   ├── base/           # 基础抽象类
│   │   ├── model/          # 核心数据模型
│   │   ├── utils/          # 工具类
│   │   ├── values/         # 常量值定义
│   │   └── widget/         # 通用UI组件
│   ├── data/               # 数据层
│   │   ├── local/          # 本地数据源
│   │   ├── model/          # 数据模型
│   │   ├── remote/         # 远程数据源
│   │   └── repository/     # 数据仓库
│   ├── modules/            # 功能模块
│   │   └── [module_name]/  # 具体功能模块
│   │       ├── bindings/   # 模块依赖绑定
│   │       ├── controllers/ # 控制器
│   │       ├── model/      # 模块数据模型
│   │       ├── views/      # 视图层
│   │       └── widgets/    # 模块专用组件
│   ├── network/            # 网络层
│   └── routes/             # 路由配置
├── flavors/                # 环境配置
├── l10n/                   # 国际化文件
├── main_dev.dart          # 开发环境入口
└── main_prod.dart         # 生产环境入口
```

### 1.3 模块内部结构规范
每个功能模块必须遵循以下结构：
- `bindings/` - 依赖注入配置
- `controllers/` - 业务逻辑控制器
- `views/` - UI视图文件
- `models/` - 模块特定数据模型（可选）
- `widgets/` - 模块专用组件（可选）

## 2. 命名约定

### 2.1 文件命名
- **Dart文件**: 使用小写字母和下划线分隔 `snake_case`
  - 示例: `home_controller.dart`, `github_repository_impl.dart`
- **目录名**: 使用小写字母和下划线分隔
  - 示例: `project_details/`, `remote_source/`
- **资源文件**: 使用小写字母和下划线分隔
  - 示例: `ic_home.svg`, `launcher_icon.png`

### 2.2 类命名
- **Controller类**: 以`Controller`结尾，使用`PascalCase`
  - 示例: `HomeController`, `SettingsController`
- **View类**: 以`View`结尾，使用`PascalCase`
  - 示例: `HomeView`, `ProjectDetailsView`
- **Model类**: 使用`PascalCase`，描述性命名
  - 示例: `GithubProjectUiData`, `GithubSearchQueryParam`
- **Repository类**: 以`Repository`结尾
  - 示例: `GithubRepository`, `GithubRepositoryImpl`
- **DataSource类**: 以`DataSource`结尾
  - 示例: `GithubRemoteDataSource`
- **Exception类**: 以`Exception`结尾
  - 示例: `NetworkException`, `ApiException`

### 2.3 变量和函数命名
- **变量**: 使用`camelCase`
  - 示例: `projectList`, `pageNumber`, `isLoadingPage`
- **私有变量**: 以下划线开头
  - 示例: `_repository`, `_githubProjectListController`
- **常量**: 使用`UPPER_SNAKE_CASE`
  - 示例: `DEFAULT_PAGE_SIZE`, `API_BASE_URL`
- **函数/方法**: 使用`camelCase`，动词开头
  - 示例: `getGithubGetxProjectList()`, `onRefreshPage()`
- **私有方法**: 以下划线开头
  - 示例: `_handleProjectListResponseSuccess()`, `_isLastPage()`

### 2.4 路由命名
- 路由常量使用`UPPER_SNAKE_CASE`
- 路径使用小写字母和连字符分隔
```dart
static const HOME = '/home';
static const PROJECT_DETAILS = '/project-details';
```

## 3. 代码风格指南

### 3.1 导入顺序
```dart
// 1. Dart核心库
import 'dart:async';

// 2. Flutter框架库
import 'package:flutter/material.dart';

// 3. 第三方包
import 'package:get/get.dart';
import 'package:logger/logger.dart';

// 4. 项目内部文件（使用相对路径）
import '/app/core/base/base_controller.dart';
import '/app/data/repository/github_repository.dart';
```

### 3.2 类结构顺序
```dart
class ExampleController extends BaseController {
  // 1. 静态常量
  static const int DEFAULT_PAGE_SIZE = 20;
  
  // 2. 实例变量（公共）
  final GithubRepository repository;
  
  // 3. 实例变量（私有）
  final RxList<Item> _items = RxList.empty();
  
  // 4. Getter方法
  List<Item> get items => _items.toList();
  
  // 5. 构造函数
  ExampleController({required this.repository});
  
  // 6. 生命周期方法
  @override
  void onInit() {
    super.onInit();
  }
  
  // 7. 公共方法
  void loadData() {
    // 实现
  }
  
  // 8. 私有方法
  void _handleSuccess(Response response) {
    // 实现
  }
}
```

### 3.3 代码格式化
- 使用`dart format`进行代码格式化
- 行长度限制为120字符
- 使用2个空格进行缩进
- 在类、方法之间添加空行
- 复杂表达式使用括号明确优先级

### 3.4 注释规范
```dart
/// 获取GitHub项目列表
/// 
/// [queryParam] 搜索查询参数
/// 返回包含项目信息的Future对象
Future<GithubProjectSearchResponse> searchProject(
  GithubSearchQueryParam queryParam,
) async {
  // 发送网络请求
  final response = await _apiClient.get('/search/repositories');
  
  return GithubProjectSearchResponse.fromJson(response.data);
}
```

## 4. 界面样式设计规范

### 4.1 设计原则
- **自然和谐**: 界面样式设计应当避免过多AI生成感，保持自然和谐的视觉效果
- **用户体验优先**: 设计应以用户体验为核心，避免过度设计和复杂效果
- **一致性**: 保持整个应用的视觉风格统一，建立清晰的设计语言
- **可读性**: 确保文字清晰易读，颜色对比度符合无障碍标准

### 4.2 视觉设计要求

#### 4.2.1 色彩搭配
```dart
// 推荐使用自然、柔和的色彩搭配
class AppColors {
  static const Color primary = Color(0xFF2196F3);     // 主色调
  static const Color secondary = Color(0xFF03DAC6);   // 辅助色
  static const Color background = Color(0xFFFAFAFA);  // 背景色
  static const Color surface = Color(0xFFFFFFFF);     // 表面色
  static const Color error = Color(0xFFB00020);       // 错误色
  
  // 避免使用过于鲜艳或人工感强烈的颜色
  // 推荐使用Material Design或Human Interface Guidelines的色彩规范
}
```

#### 4.2.2 字体和排版
```dart
// 使用系统字体或经典字体，避免过于花哨的字体
class AppTextStyles {
  static const TextStyle heading1 = TextStyle(
    fontSize: 24,
    fontWeight: FontWeight.bold,
    letterSpacing: 0.15,
    height: 1.2,
  );
  
  static const TextStyle body1 = TextStyle(
    fontSize: 16,
    fontWeight: FontWeight.normal,
    letterSpacing: 0.5,
    height: 1.5,
  );
  
  // 确保文字间距和行高适中，提升可读性
}
```

#### 4.2.3 组件设计
- **圆角设计**: 使用适度的圆角（4-12px），避免过度圆润
- **阴影效果**: 使用微妙的阴影，模拟真实物理效果
- **动画过渡**: 使用自然的缓动函数，避免生硬的动画效果

```dart
// 自然的组件样式示例
class NaturalCard extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Container(
      decoration: BoxDecoration(
        color: Colors.white,
        borderRadius: BorderRadius.circular(8), // 适度圆角
        boxShadow: [
          BoxShadow(
            color: Colors.black.withOpacity(0.08), // 微妙阴影
            blurRadius: 8,
            offset: const Offset(0, 2),
          ),
        ],
      ),
      child: // 内容
    );
  }
}
```

### 4.3 交互设计规范

#### 4.3.1 按钮设计
- 使用清晰的视觉层次区分主要和次要按钮
- 提供适当的触摸反馈，但避免过度的视觉效果
- 按钮尺寸符合人体工程学，最小触摸区域44x44dp

#### 4.3.2 动画效果
```dart
// 推荐使用Flutter内置的自然动画曲线
class NaturalAnimations {
  static const Duration defaultDuration = Duration(milliseconds: 300);
  static const Curve defaultCurve = Curves.easeInOut;
  
  // 避免使用过于夸张的动画效果
  static const Curve subtleCurve = Curves.easeInOutCubic;
}
```

### 4.4 布局设计原则

#### 4.4.1 间距系统
```dart
// 建立统一的间距系统
class AppSpacing {
  static const double xs = 4.0;
  static const double sm = 8.0;
  static const double md = 16.0;
  static const double lg = 24.0;
  static const double xl = 32.0;
  
  // 使用8dp网格系统，确保视觉协调
}
```

#### 4.4.2 内容密度
- 避免界面过于拥挤或过于稀疏
- 合理使用留白，让界面呼吸感更强
- 重要信息突出显示，次要信息适当弱化

### 4.5 避免AI生成感的具体措施

1. **避免过度完美**: 适当的不对称和微小变化让界面更自然
2. **减少渐变滥用**: 谨慎使用渐变效果，优先选择纯色
3. **控制特效使用**: 避免过多的光效、粒子等特效
4. **保持简洁**: 遵循"少即是多"的设计哲学
5. **参考现实**: 从现实世界中汲取设计灵感，而非完全依赖算法生成

### 4.6 设计审查清单

在界面设计完成后，请检查以下要点：
- [ ] 色彩搭配是否自然协调
- [ ] 字体选择是否易读且不突兀
- [ ] 组件样式是否过于人工化
- [ ] 动画效果是否自然流畅
- [ ] 整体视觉是否和谐统一
- [ ] 是否符合目标用户的审美习惯

## 5. 文档编写标准

### 5.1 代码文档
- 所有公共API必须有文档注释
- 使用`///`进行文档注释
- 包含参数说明、返回值说明、使用示例

```dart
/// 搜索GitHub项目
///
/// 根据提供的查询参数搜索GitHub上的项目。
/// 支持分页加载和关键词搜索。
///
/// [queryParam] 搜索查询参数，包含关键词和页码
/// 
/// 返回包含项目列表的[GithubProjectSearchResponse]对象
/// 
/// 示例:
/// ```dart
/// final queryParam = GithubSearchQueryParam(
///   searchKeyWord: 'flutter',
///   pageNumber: 1,
/// );
/// final result = await repository.searchProject(queryParam);
/// ```
/// 
/// 抛出:
/// * [NetworkException] 当网络连接失败时
/// * [ApiException] 当API返回错误时
Future<GithubProjectSearchResponse> searchProject(
  GithubSearchQueryParam queryParam,
) async {
  // 实现代码
}
```

### 5.2 README文档结构
```markdown
# 项目名称

## 项目简介
## 功能特性
## 技术栈
## 项目结构
## 快速开始
## 开发指南
## API文档
## 贡献指南
## 许可证
```

### 5.3 变更日志
- 使用`CHANGELOG.md`记录版本变更
- 按版本号倒序排列
- 分类记录新增、修改、修复、删除的内容

## 6. 异常处理原则

### 6.1 异常分类体系
```dart
// 基础异常类
abstract class BaseException implements Exception {
  final String message;
  final int? code;
  
  BaseException({required this.message, this.code});
}

// 网络相关异常
class NetworkException extends BaseException {
  NetworkException({required String message, int? code})
      : super(message: message, code: code);
}

// API相关异常
class ApiException extends BaseException {
  ApiException({required String message, int? code})
      : super(message: message, code: code);
}
```

### 6.2 异常处理策略
- **Controller层**: 捕获并处理所有异常，更新UI状态
- **Repository层**: 转换底层异常为业务异常
- **DataSource层**: 只处理数据源特定异常

```dart
// Controller中的异常处理
void loadData() {
  callDataService(
    _repository.getData(),
    onSuccess: (data) {
      // 处理成功情况
      _updateUI(data);
    },
    onError: (exception) {
      // 处理异常情况
      _handleError(exception);
    },
  );
}

void _handleError(Exception exception) {
  if (exception is NetworkException) {
    showErrorMessage('网络连接失败，请检查网络设置');
  } else if (exception is ApiException) {
    showErrorMessage('服务器错误：${exception.message}');
  } else {
    showErrorMessage('未知错误，请稍后重试');
  }
}
```

### 6.3 错误日志记录
```dart
// 使用Logger记录错误
try {
  final result = await apiCall();
  return result;
} catch (error, stackTrace) {
  logger.e(
    'API调用失败',
    error: error,
    stackTrace: stackTrace,
  );
  rethrow;
}
```

### 6.4 用户友好的错误提示
- 网络错误：提供重试选项
- 服务器错误：显示通用错误信息
- 验证错误：显示具体字段错误
- 权限错误：引导用户登录或授权

## 7. 消息提示规范

### 7.1 消息提示原则
- **关键性原则**: 仅对关键重要消息进行提示，避免过度使用消息提示功能
- **简洁明确**: 消息提示应当简洁明确，内容精准，避免冗长描述
- **必要性判断**: 只在必要时显示消息提示，确保用户体验不被频繁打断
- **用户体验优先**: 消息提示的频率和时机应以用户体验为核心考量

### 7.2 消息提示分类

#### 7.2.1 错误提示（必须显示）
```dart
// 网络错误、API错误等关键错误
showErrorMessage('网络连接失败，请检查网络设置');
```

#### 7.2.2 成功提示（谨慎使用）
```dart
// 仅在重要操作成功时显示
showSuccessMessage('数据保存成功');
```

#### 7.2.3 警告提示（重要操作前）
```dart
// 删除、清空等不可逆操作前的确认
showWarningDialog('确定要删除这个项目吗？此操作不可撤销。');
```

#### 7.2.4 信息提示（避免过度使用）
```dart
// 仅在用户需要了解状态变化时使用
showInfoMessage('正在同步数据...');
```

### 7.3 消息提示实现规范

#### 7.3.1 提示时长控制
```dart
// 错误提示：较长显示时间，便于用户阅读
Get.snackbar(
  '错误',
  message,
  duration: const Duration(seconds: 4),
  backgroundColor: Colors.red,
);

// 成功提示：短暂显示
Get.snackbar(
  '成功',
  message,
  duration: const Duration(seconds: 2),
  backgroundColor: Colors.green,
);
```

#### 7.3.2 提示频率限制
```dart
class MessageThrottler {
  static final Map<String, DateTime> _lastShowTime = {};
  static const Duration _throttleDuration = Duration(seconds: 3);
  
  static bool canShow(String messageKey) {
    final now = DateTime.now();
    final lastTime = _lastShowTime[messageKey];
    
    if (lastTime == null || now.difference(lastTime) > _throttleDuration) {
      _lastShowTime[messageKey] = now;
      return true;
    }
    return false;
  }
}
```

#### 7.3.3 消息优先级管理
```dart
enum MessagePriority { low, normal, high, critical }

class MessageManager {
  static void showMessage(String message, MessagePriority priority) {
    switch (priority) {
      case MessagePriority.critical:
        // 立即显示，不受限制
        _showImmediately(message);
        break;
      case MessagePriority.high:
        // 高优先级，可打断低优先级消息
        _showWithPriority(message);
        break;
      case MessagePriority.normal:
      case MessagePriority.low:
        // 普通和低优先级，需要节流控制
        if (MessageThrottler.canShow(message)) {
          _showNormal(message);
        }
        break;
    }
  }
}
```

### 7.4 消息提示最佳实践

1. **避免连续提示**: 同类型消息在短时间内不重复显示
2. **上下文相关**: 消息内容应与当前操作上下文相关
3. **可操作性**: 错误消息应提供解决方案或操作建议
4. **国际化支持**: 所有消息文本支持多语言
5. **无障碍访问**: 考虑视觉障碍用户的屏幕阅读器支持

### 7.5 消息提示禁用场景

以下情况应避免显示消息提示：
- 用户主动触发的常规操作（如点击按钮、滑动刷新）
- 后台数据同步等用户无感知操作
- 频繁发生的状态变化（如输入验证）
- 用户已经通过UI状态变化感知到的操作结果

## 8. 性能优化建议

### 8.1 内存管理
```dart
// 正确的资源释放
class ExampleController extends BaseController {
  StreamSubscription? _subscription;
  Timer? _timer;
  
  @override
  void onClose() {
    _subscription?.cancel();
    _timer?.cancel();
    super.onClose();
  }
}
```

### 8.2 网络优化
- 使用连接池复用HTTP连接
- 实现请求缓存机制
- 合理设置超时时间
- 使用压缩减少传输数据量

```dart
// Dio配置示例
class DioProvider {
  static Dio createDio() {
    final dio = Dio();
    
    dio.options = BaseOptions(
      connectTimeout: const Duration(seconds: 30),
      receiveTimeout: const Duration(seconds: 30),
      sendTimeout: const Duration(seconds: 30),
    );
    
    // 添加缓存拦截器
    dio.interceptors.add(CacheInterceptor());
    
    return dio;
  }
}
```

### 8.3 UI性能优化
- 使用`const`构造函数减少重建
- 合理使用`ListView.builder`进行列表优化
- 避免在`build`方法中进行复杂计算
- 使用`RepaintBoundary`隔离重绘区域

```dart
// 优化的列表构建
class OptimizedListView extends StatelessWidget {
  const OptimizedListView({Key? key}) : super(key: key);
  
  @override
  Widget build(BuildContext context) {
    return ListView.builder(
      itemCount: items.length,
      itemBuilder: (context, index) {
        return RepaintBoundary(
          child: ItemWidget(
            key: ValueKey(items[index].id),
            item: items[index],
          ),
        );
      },
    );
  }
}
```

### 8.4 状态管理优化
- 避免不必要的状态更新
- 使用`Obx`进行精确的局部更新
- 合理拆分Controller，避免单个Controller过于庞大

```dart
// 精确的状态更新
class HomeController extends BaseController {
  final _counter = 0.obs;
  int get counter => _counter.value;
  
  void increment() {
    _counter.value++; // 只更新依赖counter的UI
  }
}

// 在UI中使用
Obx(() => Text('计数: ${controller.counter}'))
```

### 8.5 代码分割和懒加载
- 使用GetX的懒加载特性
- 按需加载页面和资源
- 合理使用`Get.lazyPut`

```dart
// 懒加载绑定
class HomeBinding extends Bindings {
  @override
  void dependencies() {
    Get.lazyPut<HomeController>(
      () => HomeController(),
      fenix: true, // 页面销毁后可重新创建
    );
  }
}
```

### 8.6 性能监控
- 使用Flutter Inspector监控UI性能
- 集成性能监控工具（如Firebase Performance）
- 定期进行性能测试和优化

---

## 总结

本开发规范文档涵盖了Flutter GetX MVVM项目的各个方面，从文件组织到性能优化。遵循这些规范将有助于：

1. **提高代码质量**: 统一的命名和结构规范
2. **增强可维护性**: 清晰的架构分层和模块化设计
3. **提升开发效率**: 标准化的开发流程和工具配置
4. **保证项目稳定性**: 完善的测试和异常处理机制
5. **优化用户体验**: 性能优化和错误处理最佳实践

请所有开发人员严格遵循本规范，并在实践中不断完善和更新。如