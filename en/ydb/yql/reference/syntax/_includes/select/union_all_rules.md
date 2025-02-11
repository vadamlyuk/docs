---
sourcePath: en/ydb/ydb-docs-core/en/core/yql/reference/yql-docs-core-2/syntax/_includes/select/union_all_rules.md
sourcePath: en/ydb/yql/reference/yql-docs-core-2/syntax/_includes/select/union_all_rules.md
---

* The resulting table includes all columns that were found in at least one of the input tables.
* If a column wasn't present in all the input tables, then it's automatically assigned the [optional data type](../../../types/optional.md) (that can accept `NULL`).
* If a column in different input tables had different types, then the shared type (the broadest one) is output.
* If a column in different input tables had a heterogeneous type, for example, string and numeric, an error is raised.
