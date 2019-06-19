---
layout: post
title: "Azure Device Twins 與 Module Twins 的差別"
subtitle: "Azure Device Twins vs Module Twins"
date: 2019-06-18 18:00:00 +0800
author: "cain19811028"
tags:
  - Azure
---

### Azure Device Twins： 
官方說明：[Understand and use device twins in IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-device-twins)

Device Twins 是存放 Device 狀態資訊的 JSON 文件，包括 metadata、configurations 和 conditions。
使用 Device Twins 可以：

 - 在雲端儲存 Device 特定的 metadata。例如，販賣機的部署位置。
 - 從您的 Device App 報告目前狀態資訊。例如，Device 會透過 cellular 或 WiFi 連線到您的 IoT Hub。
 - 同步處理 Device 與後端應用程式之間長時間執行之工作流程的狀態。
 - 查詢 Device 的 metadata、configuration 或狀態。

### Azure Module Twins：
官方說明：[Understand and use module twins in IoT Hub](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-devguide-module-twins)

 - 在 IoT Hub 的每個 Device Identity 下，您可以建立最多 20 個 Module Identities
 - Module Identity 和 Module Twin 所提供的功能，和 Device Identity 與 Device Twin 所提供的功能相同，但是更為細微。

官方實際範例說明：您的販賣機具有三個不同的感應器。每個感應器都由公司中的不同部門控制。您可以為每個感應器建立一個模組。如此一來，每個部門只能將作業或直接方法傳送至其所控制的感應器，以避免衝突和使用者錯誤。

![](/img/in-post/2019-06-18-azure_device_twins_vs_module_twins.png)

### IoT Hub Service REST API：
API 網址：[https://docs.microsoft.com/en-us/rest/api/iothub/service](https://docs.microsoft.com/en-us/rest/api/iothub/service)
