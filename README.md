# SuperAP LSPosed Module

SuperAP is an LSPosed module designed for Android (targeted at Android 16/15) that automatically manages tethering functionality without any user interface.

## Features

- **Auto Wi-Fi Hotspot on Boot:** Automatically enables the Wi-Fi hotspot approximately 10 seconds after the system is ready.
- **Auto USB Tethering:** Automatically enables USB tethering whenever a USB host connection is detected (configured state).
- **Simultaneous Tethering:** Allows both Wi-Fi hotspot and USB tethering to be active at the same time (hardware permitting).
- **No UI:** Runs entirely in the background within the `system_server` process.

## How it works

The module hooks into `com.android.server.am.ActivityManagerService.systemReady` within the `android` (system_server) package.

1.  **System Ready:** Once the system reports it is ready, the module captures the system context.
2.  **Delayed Start:** After a 10-second delay (to ensure all network services are initialized), it attempts to enable the Wi-Fi hotspot.
3.  **USB Monitoring:** It registers a `BroadcastReceiver` for `android.hardware.usb.action.USB_STATE`. When the device enters a "connected" and "configured" state (connected to a host), it triggers the USB tethering activation.

## Requirements

- Android 11+ (Targeted at Android 15/16)
- Rooted device with LSPosed framework installed.

## Installation

1.  **Download the APK:** Go to the [Releases](https://github.com/Santuybe/SuperAP/releases) page and download the latest `app-release.apk`.
2.  **Install the APK:** Install the downloaded APK on your Android device.
3.  **Enable the Module:**
    - Open the **LSPosed Manager** app.
    - Go to the **Modules** section.
    - Find **SuperAP** and toggle the switch to enable it.
    - Ensure the "System Framework" scope is selected (it should be by default).
4.  **Reboot:** Reboot your device to apply the changes and start the automated tethering service.

## Implementation Details

- **Package:** `com.superap.module`
- **Hook Point:** `system_server` (package `android`)
- **API Used:** `ConnectivityManager` and `TetheringManager` via reflection.
- **Xposed API:** Uses standard Xposed Bridge for hooking.
