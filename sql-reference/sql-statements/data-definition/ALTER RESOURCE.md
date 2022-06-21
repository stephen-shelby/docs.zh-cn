# ALTER Resource

## 功能

该语句用于修改一个Resource的properity。

## 语法

```sql
ALTER RESOURCE table_name SET PROPERTIES ("key"="value", ...)
```

说明：

1. 当前该语句仅支持Hive/Iceberg/Hudi Resource修改其hive metastore 实例的地址，其中Hive/Hudi Resource对应的Key为`hive.metastore.uris`, Iceberg Resource对应的Key为`iceberg.catalog.hive.metastore.uris`. 同时也支持修改Iceberg Resource的`iceberg.catalog-impl`.

2. 修改Resource中的Hive Metastore地址是避免用户Hive Metastore地址修改之后重新在StarRocks创建外表。在修改`hive.metastore.uris` 前，已经引用该资源创建外部表，在修改配置后，除非在新的Hive Metastore中有存在与原Hive Metastore中对应的同名称且相同Schema的表，否则对该外表查询会失败。

## example

1. 修改名为"hive0"的Hive Resource中的Hive Metastore地址。

    ```sql
    ALTER RESOURCE 'hive0' SET PROPERTIES ("hive.metastore.uris" = "thrift://10.10.44.91:9083")
    ```
