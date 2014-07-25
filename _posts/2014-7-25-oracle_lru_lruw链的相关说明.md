---
    layout: post
    title: oracle_lru_lruw链的相关说明
    category:oracle
---

- `LRU`链是用来连接关键的数据块
- `LURW`链是用来连接脏块。

`LRU`链和`LRUW`链都是区分冷热块的链表。在`LRUW`上的脏快中的冷端上的数据库会被优先写入磁。
