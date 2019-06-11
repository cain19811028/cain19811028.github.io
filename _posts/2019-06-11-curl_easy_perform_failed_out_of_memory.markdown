---
layout: post
title: "使用 Azure IoT Python SDK 遇到 curl_easy_perform() failed: Out of memory 的問題"
subtitle: "Azure IoT Python SDK Error: curl_easy_perform() failed: Out of memory"
date: 2019-06-11 10:00:00 +0800
author: "cain19811028"
tags:
  - Azure
  - IoT
  - Python
---

### 前言：

這兩天用 Azure 官方提供的 [Azure IoT Python SDK](https://github.com/Azure/azure-iot-sdk-python) 來實作一些程式，沒想到連續遇到了一些坑 ... 不知道是官方對 Python 的支援不夠完整，還是對 MAC 的支援不夠完整 ... 總之用起來非常不順就是了。

 - MAC OS version：10.13.5
 - Python version：3.7.3

### 相關問題：

 - [https://github.com/Azure/azure-iot-sdk-c/issues/219](https://github.com/Azure/azure-iot-sdk-c/issues/219)
 - [https://github.com/Azure/azure-iot-sdk-c/issues/308](https://github.com/Azure/azure-iot-sdk-c/issues/308)
 - [https://github.com/Azure/azure-iot-sdk-c/issues/519](https://github.com/Azure/azure-iot-sdk-c/issues/519)：
