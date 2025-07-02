+++
date = '2025-07-02T09:38:09+08:00'
draft = false
title = 'Brighten Sql'
description = "项目常用sql"
tags = [
    "sql"
]
categories = [
    "sql"
]
+++

# 项目常用sql

## 销售额导出

### 汇总

```sql
select t1.SaleDate,
       SUM(t2.OrderMoney)                   AS OrderTotalAmount,
       SUM(t2.OrderMoney - t2.ReturnAmount) AS ActualAmount
from SL_Order t1
         inner join SL_OrderDetail t2 on t1.Id = t2.OrderId
where (1 = 1)
  and (t1.RecordStatus = 1 and t2.RecordStatus = 1 and t1.OrderStatus > 0)
  and (t1.SaleDate >= '2024-11-01' and t1.SaleDate <= '2024-12-01')
GROUP BY t1.SaleDate
ORDER BY t1.SaleDate
```

### 午唱、晚场

```sql
SELECT t1.SaleDate,
       SUM(t2.OrderMoney) AS '销售金额', SUM(t2.OrderMoney - t2.ReturnAmount) AS '有效金额', CASE
                                                                                                 WHEN t1.OrderType = 4
                                                                                                     THEN '晚场'
                                                                                                 ELSE '午场'
        END AS OrderType
FROM SL_Order t1
         INNER JOIN
     SL_OrderDetail t2 ON t1.Id = t2.OrderId
WHERE t1.RecordStatus = 1
  AND t2.RecordStatus = 1
  AND t1.OrderStatus > 0
  AND t1.SaleDate >= '2024-11-01'
  AND t1.SaleDate <= '2024-12-01'
GROUP BY t1.SaleDate,
         CASE
             WHEN t1.OrderType = 4 THEN '晚场'
             ELSE '午场'
             END
ORDER BY t1.SaleDate;
```