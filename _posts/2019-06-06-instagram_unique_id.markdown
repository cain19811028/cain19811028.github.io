---
layout: post
title: "用 PostgreSQL 實作 Instagram 產生 Unique ID 的方式"
subtitle: "Instagram Unique ID with PostgreSQL"
date: 2019-06-06 14:00:00 +0800
author: "cain19811028"
tags:
  - PostgreSQL
  - Unique ID
  - UUID
  - Snowflake
---
### 前言

最近一直在看各種 Unique ID 是否會重複的問題，大概比較了下列幾種：

  - Auto Increment ID
  - UUID1
  - UUID4
  - Flickr Unique ID
  - Twitter Snowflake
  - Instagram Unique ID
  - MongoDB ObjectId
  - 美團分散式 ID 生成系统 - Leaf
  - 百度 UidGenerator

各有優缺點，而且使用在大型分散式系統的狀況下，幾乎都不保證 100% 不會重複，只是遇到的機會有多低而已 ...

由於目前工作上的 DB 是直接使用 PostgreSQL，所以這次就來試試看除了 Auto Increment ID 和 UUID 之外，PRIMARY KEY 採用 Instagram Unique ID 的生成方式，來看看實際執行的結果

### 實作

Instagram 的 Unique ID 包含：

 - 41 bits Timestamp（以毫秒為單位）
 - 13 bits 表示邏輯 shard ID
 - 10 bits 代表自動遞增序列

先建立一個 sequence
```sql
CREATE SEQUENCE global_id_sequence;
```

建立 ID Generator
```sql
CREATE OR REPLACE FUNCTION id_generator(OUT result bigint) AS $$
DECLARE
  our_epoch bigint := 1557777777777;
  seq_id bigint;
  now_millis bigint;
  shard_id int := 1;
BEGIN
  SELECT nextval('global_id_sequence') % 1024 INTO seq_id;
  SELECT FLOOR(EXTRACT(EPOCH FROM clock_timestamp()) * 1000) INTO now_millis;
  result := (now_millis - our_epoch) << 23;
  result := result | (shard_id << 10);
  result := result | (seq_id);
END;
$$ LANGUAGE PLPGSQL;
```

建立 Table，並且將 ID 的預設值，設定成由 ID Generator 來產生 
```sql
DROP TABLE IF EXISTS person;
CREATE TABLE person (
    id bigint PRIMARY KEY DEFAULT id_generator(),
    name varchar(30),
    created_at timestamptz DEFAULT now()
);
```

新增一筆資料來測試回傳的 Unique ID
```sql
INSERT INTO person(name) VALUES ('Cain') RETURNING id;
```

| id  |
| --- |
| 16989044114195462 |

### 心得：

單純從 PostgreSQL PRIMARY KEY 的角度來看：

 - Auto Increment ID：
   - 優點：
     1. 簡單、好記
     2. 方便排序
   - 缺點：
     1. 分散式系統下無法確保唯一性
     2. 分表或需要合併時會顯得比較麻煩
     3. 容易有安全性的問題（例：容易看出有多少用戶，並且可以隨機亂測使用者代碼）

 - UUID：
   - 優點：
     1. 本地生成，速度快，不需用到網路 IO
     2. Data Migration 時通常比較不會有問題
   - 缺點：
     1. 不易閱讀
     2. 沒有順序性
     3. 佔用較大空間
     4. String 查詢效率普遍較差

 - Instagram Unique ID：
   - 優點：
     1. 純數字
     2. 可排序
     3. 利用 logic Shard 改善 Snowflake Worker 的問題，達到去中心化
   - 缺點：
     1. 只能用 69 年！？ 41 bits 的 Timestamp 可以使用 (1L << 41) / (1000L 60 60 24 365) = 69年；

### 參考來源：

 - Sharding & IDs at Instagram：[https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)