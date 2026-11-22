# 小票（标签）蓝牙打印机安卓 SDK

面向 Xcprinter BLE 打印机的本地 Android Library

## 主要功能：
* 打印设备扫描
* 连接与断开
* 发送打印内容
* MTU 协商，Credits 流控


## 如何使用

1. 将 AAR 文件复制到目标项目，例如：
```
app/libs/bluetooth-sdk-release.aar
```

2. 在目标项目的 app/build.gradle.kts 中添加：
```
dependencies {
  implementation(files("libs/bluetooth-sdk-release.aar"))
  implementation("androidx.core:core-ktx:1.15.0")
}
```

3. 集成

在宿主的 `settings.gradle.kts` 中添加依赖：

```kotlin
dependencies {
  implementation(files("xcprinter.aar"))
}
```

SDK 的 manifest 会合并蓝牙权限及可选 BLE 硬件声明。宿主仍负责在合适的界面请求 `BluetoothPermissions.required()` 返回的运行时权限。

## 使用

在 `Application.onCreate()` 初始化一次：

```kotlin
BluetoothPrinterManager.initialize(this)
BluetoothPrinterManager.isDebugLoggingEnabled = BuildConfig.DEBUG
```

设置回调并执行操作：

```kotlin
BluetoothPrinterManager.onDeviceFound = { device ->
  // device.name / device.address，回调统一在主线程执行
}
BluetoothPrinterManager.onConnected = { success -> }
BluetoothPrinterManager.onDisconnected = { success -> }
BluetoothPrinterManager.onDataSent = { result -> }

BluetoothPrinterManager.startScan()
BluetoothPrinterManager.connect(address)
BluetoothPrinterManager.send(bytes)
BluetoothPrinterManager.disconnect(address)
```

`startScan()` 扫描 5 秒后自动停止，仅上报有名称且不重复的设备。`send()` 按协商后的包长切片，收到 Credits 后自动发送；每次调用的全部包写完后回调一次 `onDataSent`。连接断开后，未完成的调用会收到失败回调。

宿主可在页面销毁时清空自己设置的回调。SDK 不负责 Hotwire 消息转换、设备偏好存储、Activity/Fragment 生命周期和权限弹窗。
