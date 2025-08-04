# 📱 AppsFlyer WebView HTTP Sample Page

This repository demonstrates how to prevent **duplicate in-app event reporting** when integrating the [AppsFlyer PBA Web SDK](https://support.appsflyer.com/hc/en-us/articles/360001610038-PBA-Web-SDK-integration-guide) inside a **hybrid mobile WebView app**.  
It contains two example HTML files that handle the following scenarios:

- **`jsinterface.html`**: WebView with JavaScript interface injection (used by Android/iOS AppsFlyer SDKs)
- **`urlloading.html`**: WebView with URL loading event capture (used by Android/iOS AppsFlyer SDKs)

Each sample demonstrates how to detect whether the mobile SDK is already present and prevent the Web SDK from initializing, avoiding double counting of events.

---


## 🔗 Live Demo
You can view and test the sample WebView pages live [here](https://appsflyersdk.github.io/webview-http-sample-page/).

## 🔧 Prerequisites

- Read: [In-app events for hybrid apps — AppsFlyer Support](https://support.appsflyer.com/hc/en-us/articles/207031976-In-app-events-for-hybrid-apps)
- Set up a **hybrid mobile app** with either of the following SDK integration methods:
  - **JS Interface injection**
  - **URL loading callback**
- Retrieve your **PBA Web App Dev Key**
- Reference the official [PBA Web SDK Integration Guide](https://support.appsflyer.com/hc/en-us/articles/360001610038-PBA-Web-SDK-integration-guide)

---

## 📁 File Overview

| File | Description |
|------|-------------|
| `index.html` | Entry page to choose either JS Interface or URL loading sample |
| `jsinterface.html` | Demonstrates JS interface injection method (Android: `app`, iOS: `webkit`) and how to prevent Web SDK initialization if the SDK is already present in WebView |
| `urlloading.html` | Demonstrates the use of `af-event://` custom URL loading mechanism to pass in-app events from WebView to native code, with Web SDK fallback suppression logic |

---

## 🤖 Native Mobile SDK Repositories

- **Android (WebView with JS Interface or URL loading):**  
  https://github.com/AppsFlyerSDK/appsflyer-android-webview-sample-app
- **iOS (WKWebView with JS Interface or URL loading):**  
  https://github.com/AppsFlyerSDK/appsflyer-ios-webview-sample-app

---

## ⚙️ Runtime Behavior

### JS Interface Method

```js
if (typeof webkit !== "undefined") {
  // iOS SDK is injected
} else if (typeof app !== "undefined") {
  // Android SDK is injected
} else {
  // Web SDK can be initialized
}
```

### URL Loading Method

```js
iframe.setAttribute("src", "af-event://inappevent?eventName=...&eventValue=...");
```

This URL is intercepted by the native WebView handler and sent to the SDK.

```js
if (!(/iOS_WebView|Android_WebView/.test(navigator.userAgent))) {
  // Safe to initialize Web SDK
}
```

---

## 🧭 In-App Event Propagation Flow (Text Version)

```
┌────────────────────┐
│   In-app event     │
└─────────┬──────────┘
          │
          ▼
┌──────────────────────────────┐
│ WebView with native SDK?     │───────────┐
└──────────┬───────────────────┘           │
           │ Yes                           │ No
           ▼                               ▼
┌──────────────────────┐     ┌──────────────────────────┐
│ Invoke via JS        │     │ Initialize PBA Web SDK   │
│ interface or         │     └─────────────┬────────────┘
│ URL loading          │                   ▼
└──────────┬───────────┘     ┌──────────────────────────┐
           ▼                 │ Track event with         │
┌──────────────────────┐     │ Web SDK                  │
│ Android / iOS SDK    │     └─────────────┬────────────┘
└──────────┬───────────┘                   ▼
           ▼                 ┌──────────────────────────┐
┌──────────────────────┐     │ Send in-app event to     │
│ Send in-app event to │     │ AppsFlyer                │
│ AppsFlyer            │     └──────────────────────────┘
└──────────────────────┘
```

---

## 📎 Related Docs

- [PBA Web SDK Integration Guide](https://support.appsflyer.com/hc/en-us/articles/360001610038-PBA-Web-SDK-integration-guide)  
- [Hybrid App In-App Event Tracking](https://support.appsflyer.com/hc/en-us/articles/207031976-In-app-events-for-hybrid-apps#url-loading)

---
