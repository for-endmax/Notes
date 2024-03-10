# 重新认识MySQL

连接方式：共享内存、TCP/IP、命名管道、Unix域套接字

客户端处理请求：

- 连接管理（连接池）
- 解析与优化（查询缓存、语法解析、查询优化）-> 生成执行计划
- 存储引擎

显示存储引擎：`SHOW ENGINES;`

```sql
# 创建指定存储引擎的表
CREATE TABLE 表名(
    建表语句;
) ENGINE = 存储引擎名称;

# 修改表的存储引擎
ALTER TABLE 表名 ENGINE = 存储引擎名称;
```

# MySQL的启动选项和系统变量

命令行中使用  `--启动选项1[=值1] --启动选项2[=值2] ... --启动选项n[=值n]`

配置文件中使用

```sql
[server]
(具体的启动选项...)

[mysqld]
(具体的启动选项...)

[mysqld_safe]
(具体的启动选项...)

[client]
(具体的启动选项...)

[mysql]
(具体的启动选项...)

[mysqladmin]
(具体的启动选项...)

```

查看系统变量：`SHOW [GLOBAL|SESSION] VARIABLES [LIKE 匹配的模式];`

系统变量的范围有GLOBAL和SESSION

设置：`SET [GLOBAL|SESSION] 系统变量名 = 值;`

>  默认是查看/设置 SESSION范围的

状态变量：关于程序运行的变量

查看：`SHOW [GLOBAL|SESSION] STATUS [LIKE 匹配的模式];`

# 字符集和比较规则

