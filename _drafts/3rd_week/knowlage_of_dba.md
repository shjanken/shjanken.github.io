`sqlplus` 的相关显示设置
------------

- 使用 `set sqlprompt` 命令显示定制`sqlplus`的提示符:

        sql > set sqlprompt `&_user@&_connect_identifier> `

    使用`define` 可以查看当前的常量


- 格式化`sqlplus`的显示格式;

        set linsize 400;
        column employee_id format a20
        column first_name format a20

        column column_name clear --清除所有以设置的列属性
        column columns --清楚所有列的属性
        

`oracle` 的内存信息和物理存储信息
----------------

- 查看 `oracle sga` 各个组件的大小

        select component,current_size from v$sga_dynamic_components;
        select * from v$sgainfo;

- 查看　`shared pool` 的命中率

        select to_char(sum(pins-reloads)/sum(pins)*100,'99.99')||'%' from v$librarycache;

- 参数文件

        show parameter spfile;
        //如果没有输出则说面使用的是 pfile 文件。
        //　不然会显示　spfile 的文件路径

- 控制文件
    
        show parameter control

- 数据文件的位置

        select file_name from dba_data_files;


- 日志文件的位置

        select group#,member from v$logfile;

- 归档文件的位置

        show parameter recovery

ORACLE企业管理器
---

`oracle em` 的重新安装

    １. 删除原来的企业管理器

            $ emca -deconfig dbcontrol db
            $ emca -repos drop

    2. 重新安装企业管理器

            $ emca -repos create
            $ emca -config dbcontrol db

        **注意！安装企业管理器需要`sysman`和`sys`用户的密码，所以如果该用户没有启用，需要先启用这两个用户**


      3. 启动 `oracle em`

          1. 启动数据库
          2. 启动监听
          3. emctl start dbconsole;

    4. 启动 `lisnter` :
