有些库会在索引里使用表达式、排序方式或数据库特有选项。metadata 和数据库反射出来的索引实际等价，但 Alembic autogenerate 仍会反复生成 drop/create；现在只能用 `include_object` 把这类索引整个排除，真正的索引变化也一起看不到了。

希望 autogenerate 增加一个和 `compare_type` 类似的自定义索引比较入口。回调能够看到迁移上下文、数据库侧索引和 metadata 侧索引，并可选择沿用内置判断、忽略这次差异或明确要求重建。它只影响已经匹配到的索引比较，索引新增、删除、重命名以及现有 include 过滤仍按原规则处理。

`compare_metadata`、`produce_migrations`、`revision --autogenerate` 和 `alembic check` 应使用同一判断结果，批处理模式和常用方言不能因此出现不同语义。请补上公开配置说明和相应测试。
