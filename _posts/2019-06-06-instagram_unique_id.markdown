---
layout: post
title: "用 PostgreSQL 實作 Instagram 產生 Unique ID 的方式"
subtitle: "Instagram Unique ID with PostgreSQL"
date: 2019-06-06 14:00:00 +0800
author: "cain19811028"
tags:
  - PostgreSQL
  - Unique ID
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

### 參考來源：

 - Sharding & IDs at Instagram：[https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)