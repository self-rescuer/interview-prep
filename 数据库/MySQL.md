# MySQL 面试题

## 1. 查询年龄大于 20 的用户

```sql
SELECT * FROM user WHERE age > 20;
```

## 2. 索引是什么？为什么能加快查询？

索引是数据库的目录结构，因为它可以避免全表扫描，快速定位数据。

## 3. 索引越多越好吗？为什么？

不是。因为索引会占用磁盘空间，增删改时需要维护索引，拖慢写入速度。经常查询的字段适合建索引，频繁写的字段不适合。

## 4. 事务的 ACID 是什么？

- **原子性**：要么全成功，要么全失败
- **一致性**：事务前后数据状态一致
- **隔离性**：多个事务互不干扰
- **持久性**：提交后永久保存

## 5. INNER JOIN 和 LEFT JOIN 的区别？

- **INNER JOIN**：返回两边匹配的数据
- **LEFT JOIN**：保留左边全部数据，右边不匹配的则为 NULL

## 6. WHERE 和 HAVING 的区别？

- **WHERE**：过滤初始行，不能用聚合函数
- **HAVING**：过滤分组之后的结果，作用在 GROUP BY 之后

## 7. 不加 WHERE 的 UPDATE 会发生什么？

更新表中所有行。生产环境的高危操作，可能一行 SQL 改掉全表数据。

## 8. 怎么查询一个表中第二大的值？

```sql
SELECT MAX(字段) FROM 表名
WHERE 字段 < (SELECT MAX(字段) FROM 表名);
```

## 9. 怎么删除重复数据只保留一条？

```sql
DELETE FROM users
WHERE id NOT IN (
    SELECT MIN(id) FROM users GROUP BY email
);
```

## 10. 慢查询怎么排查？

1. 开启慢查询日志，定位慢 SQL
2. 用 `EXPLAIN` 查看执行计划
3. 分析是否用到索引、扫描了多少行
4. 优化：加索引、改写 SQL、分库分表
