---
layout: post
title: oracle checkpoint 1
catagroy: oracle
---

# ORACLE CHECKPOINT 1：概念和介绍

标签（空格分隔）： oracle 体系结构 51cto 学习笔记

---

检查点队列的相关概念：
----

1. *检查点队列*是一个链表结构

2. *检查点队列*上按照每个数据库第一次被更改的时间顺序链接了数据块。
    > 按照每个数据块第一次被脏的时间点挂到检查点队列上 

3. 由于 *检查点队列* 是按照时间顺序链接的数据块，则*检查点队列* 上所有的数据块对应的日志地址都顺序的存放在日志中。
    > *检查点队列* 上所有数据块对应的日志地址都在链上第一个数据块对应的日志之后。

4. 完全检查点和增量检查点：
    1. 完全检查点: 会将*检查点队列* 上所有的脏快写到磁盘上。
    2. 增量检查点: 将*检查点队列* 上第一个脏快对应的日志地址写到控制文件中。
    完全检查点会在关闭数据库时发生。 增量检查点是每隔3秒钟发生一次。


查询语句
---

    select cpdrt,cplrba_seq||'.'||cplrba_bno||'.'||cplrba_bof "low RBA", cpodr_seq||'.'||cpodr_bno||'.'||cpodr_bof "on disk rba",cpods,cpodt,cphbt from x$kcccp;
    
    -- cpdrt ：检查点队列种的脏快数据
    -- cpods ： on disk rba 的 scn
    -- cpodt ： on disk rba 的时间戳
    -- cphbt : 心跳 
    -- lowrba : 检查点队列中最早脏的数据块地址。
    -- on disk rba: 是`current` 日志文件的最后一条日志的地址，也就是 `log wirter` 进程写到 `redo log` 中的最后一条日志的地址。
