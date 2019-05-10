---
title: What’s New in TiDB 3.0.0-rc.1
date: 2019-05-10
author: ['段兵']
summary: 2019 年 5 月 10 日，TiDB 3.0.0-rc.1 版本正式推出，该版本对系统稳定性，性能，安全性，易用性等做了较多的改进，本文会逐一介绍。
tags: ['TiDB']
---


2019 年 5 月 10 日，TiDB 3.0.0-rc.1 版本正式推出，该版本对系统稳定性，性能，安全性，易用性等做了较多的改进，接下来逐一介绍。

## 提升系统稳定性

众所周知，数据库的查询计划的稳定性至关重要，此版本采用多种优化手段促进查询计划的稳定性得到进一步提升，如下：

1. 新增 Fast Analyze 功能，使 TiDB 收集统计信息的速度有了数量级的提升，对集群资源的消耗和生产业务的影响比普通 Analyze 方式更小。

2. 新增 Incremental Analyze 功能，对于值单调增的索引能够更加方便和快速地更新其统计信息。

3. 在 CM-Sketch 中新增 TopN 的统计信息，缓解因为 CM-Sketch 哈希冲突导致估算偏大的问题，使代价估算更加准确。

4. 优化 Cost Model，利用和 RowID 列之间的相关性更加精准的估算谓词的选择率，使得索引选择更加稳定和准确。

## 提升系统性能

1. `TableScan,IndexScan,Limit` 算子，进一步提升 SQL 执行性能。

2. TiKV 采用`Iterator Key Bound Option`存储结构减少内存分配及拷贝，RocksDB 的 Column Families 共享 block cache 提升 cache命中率等手段大幅提升性能。

3. TiDB Lightning encode SQL 性能提升 50%，将数据源内容解析成 TiDB 的 types.Datum，减少 encode 过程中多余的解析工作，使得性能得到较大的提升。

## 增强系统安全性

RBAC（Role-Based Access Control）基于角色的权限访问控制是商业系统中最常见的权限管理技术之一，通过 RBAC 思想可以构建最简单”用户-角色-权限“的访问权限控制模型。RBAC 中用户与角色关联，权限与角色关联，角色与权限之间一般是多对多的关系统，用户通过成为什么样的角色获取该角色所拥有的权限，达到简化权限管理的目的，通过此版本的迭代 RBAC 功能开发完成，欢迎试用。

## 提升产品易用性

1. 新增 SQL 方式查询慢查询，丰富 TiDB 慢查询日志内容，如：Coprocessor 任务数，平均/最长/90% 执行/等待时间，执行/等待时间最长的 TiKV 地址，简化慢查询定位工作，提升产品易用性。

2. 新增系统配置项合法性检查，优化系统监控项等，提升产品易用性。

3. 支持对 `TableReader`、`IndexReader` 和 `IndexLookupReader` 算子进行内存追踪控制，对 Query 内存使用统计更加精确，可以更好地检测、处理对内存消耗较大的语句。

## 社区贡献

V3.0.0-rc.1 版本的开发过程中，开源社区贡献者给予了我们极大的支持，例如美团的同学负责开发的 [SQL Plan Management](https://github.com/pingcap/tidb/projects/19) 特性对于提升产品的易用性有很大的帮助，一点资讯的陈付同学与其他同学一起对 TiKV 线程池进行了重构，提高了性能并降低了延迟，掌门科技的聂殿辉
同学实现 TiKV 大量 UDF 函数帮忙 TiKV 完善 Coprocessor 功能，就不再一一列举。在此对各位贡献者表示由衷的感谢。接下来我们会开展更多的专项开发活动以及一系列面向社区的培训课程，希望能对大家了解如何做分布式数据库有帮助。


