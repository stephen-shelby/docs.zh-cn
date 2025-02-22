# StarRocks version 2.1

## 2.1.0

发布日期： 2022年2月24日

### New Features

- 【公测中】支持通过外表的方式查询 Apache Iceberg 数据湖中的数据，帮助您实现对数据湖的极速分析。TPC-H 测试集的结果显示，查询 Apache Iceberg 数据时，StarRocks 的查询速度是 Presto 的 **3 - 5** 倍。相关文档，请参见 [Apache Iceberg 外表](../using_starrocks/External_table.md/#apache-iceberg外表)。
- 【公测中】发布 Pipeline 执行引擎，可以自适应调节查询的并行度。您无需手动设置 session 级别的变量 parallel_fragment_exec_instance_num。并且，在部分高并发场景中，相较于历史版本，新版本性能提升两倍。
- 支持 CTAS（CREATE TABLE AS SELECT），基于查询结果创建表并且导入数据，从而简化建表和 ETL 操作。相关文档，请参见 [CREATE TABLE AS SELECT](../sql-reference/sql-statements/data-definition/CREATE%20TABLE%20AS%20SELECT.md)。
- 支持 SQL 指纹，针对慢查询中各类 SQL 语句计算出 SQL 指纹，方便您快速定位慢查询。相关文档，请参见 [SQL 指纹](../administration/Query_planning.md/#sql指纹)。
- 新增函数 [ANY_VALUE](../sql-reference/sql-functions/aggregate-functions/any_value.md)，[ARRAY_REMOVE](../sql-reference/sql-functions/array-functions/array_remove.md)，哈希函数 [SHA2](../sql-reference/sql-functions/encryption-functions/sha2.md)。

### Improvement

- 优化 Compaction 的性能，支持导入 10000 列的数据。
- 优化 StarRocks 首次 Scan 和 Page Cache 的性能。通过降低随机 I/O ，提升 StarRocks 首次 Scan 的性能，如果首次 Scan 的磁盘为 SATA 盘，则性能提升尤为明显。另外，StarRocks 的 Page Cache 支持直接存放原始数据，无需经过 Bitshuffle 编码。因此读取 StarRocks 的 Page Cache 时无需额外解码，提高缓存命中率，进而大大提升查询效率。
- 支持主键模型（Primary Key Model）变更表结构（Schema Change），您可以执行 `ALTER TABLE` 增删和修改索引。
- 【公测中】支持字符串最大长度为 1MB。
- 优化 JSON 导入性能，并去除了 JSON 导入中单个 JSON 文件不超过 100MB 大小的限制。
- 优化 Bitmap Index 性能。
- 优化通过外表方式读取 Hive 数据的性能，支持 Hive 的存储格式为 CSV。
- 支持建表语句的时间戳字段定义为 DEFAULT CURRENT_TIMESTAMP。
- 支持导入带有多个分隔符的 CSV 文件。

### Bugfix

- 修复在导入 JSON 格式数据中设置了 jsonpaths 后不能自动识别 __op 字段的问题。
- 修复 Broker Load 导入数据过程中因为源数据发生变化而导致 BE 节点挂掉的问题。
- 修复建立物化视图后，部分 SQL 语句报错的问题。
- 修复 Routine Load 中使用带引号的 jsonpath 会报错的问题。
- 修复查询列数超过 200 列后，并发性能明显下降的问题。

### Behavior Change

修改关闭 Colocation Group 的 HTTP Restful API。为了使语义更好理解，关闭 Colocation Group 的 API 修改为 `POST /api/colocate/group_unstable`（旧接口为 `DELETE /api/colocate/group_stable` ）。

> 如果需要重新开启 Colocation Group ，则可以使用 API `POST /api/colocate/group_stable`。

### Others

flink-source-connector 支持 Flink 批量读取 StarRocks 数据，实现了直连并行读取 BE 节点、自动谓词下推等特性。相关文档，请参见 [Flink Connector](../unloading/Flink_connector.md)。
