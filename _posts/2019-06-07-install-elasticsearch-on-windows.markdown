---
layout: post
title: "在 Windows 10 上安裝 Elasticsearch"
subtitle: "Install Elasticsearch on Windows"
date: 2019-06-07 11:00:00 +0800
author: "cain19811028"
tags:
  - Elasticsearch
  - AWS
  - Azure
---
### 前言：

最近又想試寫個小應用需要用到搜尋功能，以往都是用 Lucene 和 Solr 來實作，而 Elasticsearch 也已經成名已久，一直沒機會來試玩看看，現在總算可以來踏出第一步！

### 官方資訊：

 - 官網：[https://www.elastic.co/](https://www.elastic.co/)
 - Elasticsearch 下載頁：[https://www.elastic.co/downloads/elasticsearch](https://www.elastic.co/downloads/elasticsearch)

### 安裝：

這次先裝在家裡老舊 Windows 桌機來跑跑看，Windows 的安裝方式可以選下載壓縮檔或使用 .msi 安裝，另外也有直接提供 [Docker Image](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)；這次先選擇無腦的安裝方式，下載 v7.1.1 的 .msi 安裝檔，一直按下一步，一下子就順利安裝完成！

安裝完成後直接在瀏覽器連至：[http://localhost:9200/](http://localhost:9200/)，就能看到 Elasticsearch 相關資訊

![](/img/in-post/2019-06-07-install-elasticsearch-on-windows-01.png)

### 後記：

結果裝好後沒多久，在網上就爬文爬到 AWS 和 Azure 都有相關線上的服務，Amazon Elasticsearch Service 直接寫明：免費方案客戶提供每月多達 750 小時的 t2.small.elasticsearch 執行個體和每月 10 GB 的選用 EBS 儲存免費用量。Azure Search 雖然好像也有免費方案可用，但條件限制比 AWS 多一些，有空再來深入研究好好比對一下！

 - [Amazon Elasticsearch Service](https://aws.amazon.com/tw/elasticsearch-service/)：全託管、可擴展且安全可靠的 Elasticsearch Service
 - [Azure Search](https://azure.microsoft.com/zh-tw/services/search/)：適用於行動應用程式及 Web 應用程式開發並採用 AI 技術的雲端搜尋服務