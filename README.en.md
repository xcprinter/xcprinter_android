# Android SDK for Receipt/Label Bluetooth Printers

A local Android library for Xcprinter BLE printers.

## Key Features

* Printer discovery
* Connecting and disconnecting
* Sending print content
* MTU negotiation and credit-based flow control

## How to Use

1. Copy the AAR file into the target project, for example:

```
app/libs/bluetooth-sdk-release.aar
```

2. Add the following to the target project's `app/build.gradle.kts`:

```
dependencies {
  implementation(files("libs/bluetooth-sdk-release.aar"))
  implementation("androidx.core:core-ktx:1.15.0")
}
```

3. Integration

Add the dependency to the host application's `settings.gradle.kts`:

```kotlin
dependencies {
  implementation(files("xcprinter.aar"))
}
```

The SDK manifest merges the Bluetooth permissions and the optional BLE hardware declaration. The host application is still responsible for requesting the runtime permissions returned by `BluetoothPermissions.required()` from the appropriate UI.

## Usage

Initialize once in `Application.onCreate()`:

```kotlin
BluetoothPrinterManager.initialize(this)
BluetoothPrinterManager.isDebugLoggingEnabled = BuildConfig.DEBUG
```

Set callbacks and perform operations:

```kotlin
BluetoothPrinterManager.onDeviceFound = { device ->
  // device.name / device.address; callbacks are always invoked on the main thread
}
BluetoothPrinterManager.onConnected = { success -> }
BluetoothPrinterManager.onDisconnected = { success -> }
BluetoothPrinterManager.onDataSent = { result -> }

BluetoothPrinterManager.startScan()
BluetoothPrinterManager.connect(address)
BluetoothPrinterManager.send(bytes)
BluetoothPrinterManager.disconnect(address)
```

`startScan()` automatically stops after 5 seconds and reports only named, non-duplicate devices. `send()` splits data according to the negotiated packet size, sends packets automatically when Credits are received, and invokes `onDataSent` once after all packets from each call have been written. If the connection is closed, pending calls receive a failure callback.

The host application may clear its own callbacks when a page is destroyed. The SDK does not handle Hotwire message conversion, device preference storage, Activity/Fragment lifecycles, or permission dialogs.
