# ORACLE SCN

标签（空格分隔）： oracle 学习笔记 51cto 体系结构

---

相关概念
---

1. `SCN` 是经过计算的时间戳。

2. 常见的 `SCN`：

    - 在控制文件中:
        - 系统 `SCN` ：在数据库启动时记录
        
                select checkpoint_change# from v$database;
                
        - 文件 `SCN`
        
                select name,checkpoint_change# from v$datafile;
                
        - 结束 `SCN`: 在数据库正常关闭时记录
        
                select name,last_change# from v$datafile
            
    - 在数据文件头部
        - 开始 `SCN` ：在数据库启动时记录。
        
                select name,last_change# from v$datafile_header;
    
3. 每个日志条目都有一个`SCN`
4. 每个日志文件的头部都有`SCN`号，分别是`First` 和 `Next`。
        
SCN 的意义
----
1. 在数据库启动时，`SCN` 应该保持一致。
1. 如果数据库没有正常关闭数据库的话，结束`SCN`为空。当`oracle`再次启动时会自动开始实例恢复。
2. 如果`oracle`发现某个文件的`SCN`号过旧，就会对该文件应用日志。
1. 系统,文件,和头部的 `SCN` 应该等于最旧的,状态为`active` 的`redo log` 的`SCN(First SCN)`. 当系统崩溃之后, `oracle` 按照这个`scn` 开始恢复日志.
