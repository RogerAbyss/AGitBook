# 中国囍联

* [项目规范详细文档.pdf](https://github.com/RogerAbyss/AGuidelineForIOS.git)
* [组件核心-路由器](https://github.com/RogerAbyss/ARouter)
* [自动化核心-AFastlane](https://github.com/RogerAbyss/AFastlane)

---

## 新入员工前置技术

- **Objctive-C** (主要/必要,主要开发语言)
- **xcode** (主要/必要,主要开发工具)
- Swift      (次要,部分混合开发, **注意Swift版本升级带来的兼容问题**)
- **git**        (重要, 多分支并行, 请管理好git, 多问多备份)
- **Ruby**       (重要,比Cocopods更深的了解是必要的, fastlane相关都是ruby)
- node       (次要, git已经hook,**请按项目规范完成项目,否则您可能无法提交**)
- teamcity   (次要, 比Jenkins简单好用的持续集成工具)
- sourcetree (次要, 推荐的git管理工具)
- **github** (次要, 每个开发人员都应该提升自己)

**开始项目前, 请留神历史遗留, 毕竟经历了三年的不健康迭代**

## 开始项目

```zsh
# 检查必备环境 如果没有自己安装
ruby -v
gem -v
bundler -v
node -v
npm -v
# 安装gemfile, 注意谨慎使用update
bundle install
# 安装podfile, 注意谨慎使用update
pod install
# 安装packages.json, 注意谨慎使用update
npm install
# 安装开发/发布证书, 如果无法访问联系iOS负责人添加权限
fastlane refresh
```


## 项目概述

```
.
├── CHANGELOG.md
├── Gemfile
├── Gemfile.lock
├── MANAGER.md
├── Podfile
├── Podfile.lock
├── Pods
├── README.md
├── SVProgressHUD.bundle
├── devices.txt
├── email.erb.html
├── fastlane
├── jazzy.yaml
├── package-lock.json
├── package.json
├── push
├── report-git.txt
├── testappdomain
├── testappdomain.entitlements
├── testappdomain.xcodeproj
└── testappdomain.xcworkspace
```

### 三方库

核心技术:
CocoPods

相关目录:
```
├── Podfile
├── Podfile.lock
```
因历史遗留问题, 三方库全部采用CocoPod。
以后尽量使用Carthage。
### Docs

核心技术:
jazzy

相关目录:
```
├── docs
├── jazzy.yaml
```
前期没有规划, 尽量使用。

### CI
核心技术:
fastlane

相关目录:
```
├── Gemfile
├── Gemfile.lock
├── email.erb.html
├── fastlane
├── devices.txt
├── report-git.txt
```

fastlane托管了iOS开发证书/发布证书/Adhoc证书/推送证书/编译/发布/通知
等的全部流程, 请关注fastlane下, 各个位置(邮件模块, 编译模块等)。

具体参考[AFastlane](https://github.com/RogerAbyss/AFastlane)

- [x] 请使用fastlane refresh管理证书
- [x] 请使用fastlane scan进行单元测试
- [x] 请使用fastlane test编译发布测试版
- [x] 请使用fastlane beta编译发布预发布版
- [x] 请使用fastlane release编译正式服上传Appstore

### 推送证书
核心技术:
fastlane

相关目录:
```
├── push
```

### ChangeLog
核心技术:
commitizen
husky

相关目录:
```
├── package.json
├── package-lock.json
```

git提交(gitcommit 已经hook)
```
git cz
```

生成ChangeLog
```
npm run changelog
```
## 历史遗留
```
三年强行迭代的遗留问题, 你最好小心维护。
```
### Pod三方库有本地修改,

但是通常我们的Pod库并不推送到git库。

- MMPopupView
```Objective-C
# MMPopupWindow.visiable
# MMPopupWindow.h 
@property (nonatomic, assign) BOOL visiable;
# MMAlertView.detailLabel
# MMAlertView.h
@property (nonatomic, strong) UILabel     *detailLabel;
```
- MJRefresh
```
# MJRefreshConst.h 
// 文字颜色
#define MJRefreshLabelTextColor MJRefreshColor(180, 180, 180)
// 字体大小
#define MJRefreshLabelFont [UIFont boldSystemFontOfSize:11]
```

### 页面采用的兼容模式
意味着, 大多数高分辨率手机屏幕是由下屏幕拉伸而成，所有布局/文字 都比通常的大

### Navigation错乱
跳转对你来说并不容易, 前期太多坑 导致跳转兼容处理异常困难。
建议使用最新的ARouter方式处理
详情参考[ARouter](https://github.com/RogerAbyss/ARouter)