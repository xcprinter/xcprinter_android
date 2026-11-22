# 小票（标签）蓝牙打印机安卓 SDK

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

3.
