## awtk-lpr-android 
--------------------------

车牌识别库(Android版本)。

> 首先感谢 [北京智云视图科技有限公司](http://www.zeusee.com/) 开源这么实用的软件，感谢[Aleyn](https://github.com/AleynP) 提供使用文档和android的工程。

### 更新说明

本项目在原来的基础之上做了如下改进：

* 1. 升级 OpenCV SDK 到 4.40。
* 2. 升级 NDK 到 r20b(否则与 AWTK 不兼容)。
* 3. 修复第二次进去 getDisplay() 返回 null，导致奔溃的问题。

### 使用方法

本项目主要配合 [awtk-mobile-plugins](https://github.com/zlgopen/awtk-mobile-plugins) 中的车牌识别插件 (lpr) 使用。使用方法如下：

> 以下方法建立在读者知道 [awtk-android](https://github.com/zlgopen/awtk-android) 的使用方法，并已经建立好 [awtk-android](https://github.com/zlgopen/awtk-android) 需要的开发环境基础之上。

#### 1. 获取 awtk-lpr-android

将 awtk-lpr-android 取到与 awtk-android 同级的目录。

```
git clone https://hub.fastgit.org/zlgopen/awtk-lpr-android.git
```

#### 2. 创建使用车牌识别插件 (lpr) 的项目，这里以 lpr demo 为例：

```
cd awtk-android

#请先修改脚本文件中的路径
source scripts/env_win.sh
python create_project.py ../awtk-mobile-plugins/lpr_build.json
```

> 请确保本地 awtk-mobile-plugins 是最新的。

#### 3. 修改配置

创建好项目之后，需要修改两处配置，先进入lprdemo目录：

```
cd build/lprdemo
```

* 3.1 修改 settings.gradle

在文件最后增加如下代码：

```
include ':openCV'
project(':openCV').projectDir = new File(settingsDir, '../../../awtk-lpr-android/openCV')

include ':ocr'
project(':ocr').projectDir = new File(settingsDir, '../../../awtk-lpr-android/ocr')
```

* 3.2 修改 build.gradle

在文件最后增加如下代码：

```
ext {
    android = [
            compileSdkVersion: 29,
            applicationId    : "com.pcl.lpr",
            minSdkVersion    : 21,
            targetSdkVersion : 29,
            versionCode      : 1,
            versionName      : "1.0",
    ]
}
```

#### 4. 编译

* 编译

```
./gradlew  build
```

* 安装

```
adb install -r ./app/build/outputs/apk/debug/app-debug.apk
```

----------------------------------------------------------------

## 原始项目的说明

### Demo 运行说明

Demo 地址： **[LPR](https://github.com/AleynP/LPR)**

打开项目肯定会报编译错误，要做以下修改：
1. 用 AS 打开项目
2. 设置项目 NDK 为 NDK-r14b
3. 先修改 CMakeLists.txt 文件， 把第 19 行修改成你本地的 OpenCV SDK 的对应路径。
4. 跟据自己的开发平台，设置 ocr 下的 build.gradle  第 18 行代码 是否要注释。
完成以上步骤后再运行项目，就没有问题了。

### 更新（2020-6-2）：

1. 更改了取景框适配问题
2. 更改成 AndroidX，使用了 Google 的 CamreaX 来做相机预览
3. 分离出 ocr Module ，为了方便其他项目导入识别功能
