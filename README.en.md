Printer Android SDK

## How to use
Copy the AAR file to the target project, for example：

`app/libs/bluetooth-sdk-release.aar`

In the target project's app/Add to build.gradle.kts:

```
dependencies {
  implementation(files("libs/bluetooth-sdk-release.aar"))
  implementation("androidx.core:core-ktx:1.15.0")
}
```
