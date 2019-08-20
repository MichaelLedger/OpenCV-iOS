# OpenCV-iOS
OpenCV Usage for iOS, Only for Personal Learning!

## 使用配置
先解压缩 `iPhone-ObjectiveC-Demo` 目录下的 `OpenCV.zip`，然后在工程目录 `iPhone-ObjectiveC-Demo` 下执行 `$ pod install` ，使用 `Xcode` 打开 `iPhone-ObjectiveC-Demo.xcworkspace` 即可。

### 使用 `zip` 包的原因
由于**Git限制文件不能超过100MB**，故使用 `LFS` 上传了 `OpenCV(V4.1.0)` 的 `zip` 压缩包。

```
# 终端操作日志
$ git push

Enumerating objects: 368, done.
Counting objects: 100% (368/368), done.
Delta compression using up to 4 threads
Compressing objects: 100% (345/345), done.
Writing objects: 100% (366/366), 143.18 MiB | 1.40 MiB/s, done.
Total 366 (delta 64), reused 0 (delta 0)
remote: Resolving deltas: 100% (64/64), done.
remote: error: GH001: Large files detected. You may want to try Git Large File Storage - https://git-lfs.github.com.
remote: error: Trace: 067c39d6a690067e007d745404061820
remote: error: See http://git.io/iEPt8g for more information.
remote: error: File iPhone-ObjectiveC-Demo/OpenCV/opencv2.framework/Versions/A/opencv2 is 332.36 MB; this exceeds GitHub's file size limit of 100.00 MB
To https://github.com/MichaelLedger/OpenCV-iOS.git
! [remote rejected] master -> master (pre-receive hook declined)
```

### 手动导入并配置OpenCV（更快捷）

```
$ pod search OpenCV

-> OpenCV (4.1.0)
OpenCV (Computer Vision) for iOS
pod 'OpenCV', '~> 4.1.0'
- Homepage: https://opencv.org/
- Source:  
https://github.com/opencv/opencv/releases/download/4.1.0/opencv-4.1.0-ios-framework.zip
- Versions: 4.1.0, 4.0.1, 4.0.0, 3.4.6, 3.4.5, 3.4.4, 3.4.3, 3.4.2, 3.4.1,
3.4.0, 3.3.1.1, 3.3.1, 3.3.0.1, 3.3.0, 3.2.0, 3.1.0.1, 3.1.0, 3.0.0,
2.4.13.6, 2.4.13.5, 2.4.13.4, 2.4.13.3, 2.4.13.2, 2.4.12.3, 2.4.12, 2.4.11,
2.4.10, 2.4.9.2, 2.4.9.1, 2.4.9, 2.4.8, 2.4.7, 2.4.6, 2.4.5, 2.4.3.2, 2.4.3
[master repo]
```

根据 `Source` 下载 `OpenCV` ，然后 `Podfile` 配置 `:path` 以导入本地`OpenCV`

```
# Uncomment the next line to define a global platform for your project
platform :ios, '8.0'

target 'iPhone-ObjectiveC-Demo' do
# Comment the next line if you don't want to use dynamic frameworks
use_frameworks!

# Pods for iPhone-ObjectiveC-Demo
# pod 'OpenCV', '~>4.1.0'
pod 'OpenCV', :path => './OpenCV' #指定podspec文件
end
```
根据 `OpenCV` 的 `pod` 配置文件 `OpenCV.podspec` 手动对工程进行配置：
```
Pod::Spec.new do |spec|
spec.name         = "OpenCV"
spec.version      = "4.1.0"
spec.summary      = "OpenCV"
spec.platform     = :ios, "8.0"
spec.description  = <<-DESC
Open Source Computer Vision Library https://opencv.org
DESC
spec.homepage     = "https://github.com/opencv/opencv"
spec.license      = "MIT"
spec.author   = { "opencv" => "https://opencv.org" }
spec.source       = { :git => "https://github.com/opencv/opencv.git", :tag => "#{spec.version}" }
# spec.source_files = "**/*.{h,m}"
spec.vendored_frameworks  = "**/*.{framework}"
spec.frameworks   = 'Accelerate', 'AssetsLibrary', 'AVFoundation', 'CoreGraphics', 'CoreImage', 'CoreMedia', 'CoreVideo', 'QuartzCore', 'UIKit', 'Foundation', 'Photos', 'Social'
spec.requires_arc   = false
spec.ios.deployment_target = '8.0'  
end
```

**除了在 `Target -> Build Phases -> Link Binary With Libraries` 中添加 `spec.frameworks` 中所有的框架，还需要添加 `libc++.tbd`.**
