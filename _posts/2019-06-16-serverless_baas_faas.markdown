---
layout: post
title: "何謂 Serverless? BaaS? FaaS?"
subtitle: "Serverless? BaaS? FaaS?"
date: 2019-06-16 22:00:00 +0800
author: "cain19811028"
tags:
  - Serverless
---

### Serverless 優點： 
 - 高度擴展：橫向擴展是完全自動的，由服務提供者所管理
 - 成本較低：只需要為執行函數所消耗的資源付費，完全沒有租用 Server 的成本
 - 無須維運：如果做好 DevOps 和 AIOps，開發者只需要關心業務本身，無需耗費過多維運時間

### Serverless = BaaS + FaaS ?
 - `BaaS（Backend as a Service，後端即服務）`：雲端服務商為開發者所提供的服務，如提供檔案儲存、資料儲存、推送訊息、身份驗證等功能，來幫助開發者快速開發應用。

 - `FaaS（Function as a Service，功能即服務）`：可以將程式運行在第三方提供的服務中，開發者只需要編修程式即可，不需關注 Server 狀態，而程式的執行它是由事件所觸發。例如：AWS Lambda, Azure Functions

![](/img/in-post/2019-06-16-serverless_baas_faas.png)
圖片來源：https://blog.neap.co/the-serverless-series-what-is-serverless-d651fbacf3f4